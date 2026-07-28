# TikZJax 插件绘图指南

本文档是在 Obsidian 中使用 [obsidian-tikzjax](https://github.com/artisticat1/obsidian-tikzjax) 插件绘制 LaTeX/TikZ 图形的**权威依据**。编写任何 `tikz` 代码块前务必对照。

## 0. 插件基础

- 插件渲染 ` ```tikz ` 代码块中的 LaTeX/TikZ 内容，可绘制函数图像、几何图、电路图、化学结构式、交换图等。
- 文档类固定为 `standalone`（自动应用，无需手写 `\documentclass`）。
- **必须**包含 `\begin{document}` 与 `\end{document}`，缺一不可。
- 需要宏包时，`\usepackage{}` 写在 `\begin{document}` **之前**。

### 支持的宏包（仅限这些）

`chemfig`、`tikz-cd`、`circuitikz`、`pgfplots`、`array`、`amsmath`（含 amstext）、`amsfonts`、`amssymb`、`tikz-3dplot`

加载不支持的宏包会导致整块渲染失败。基础 `tikzpicture` 环境无需任何 `\usepackage`。

## 1. 最小模板

````
```tikz
\begin{document}
\begin{tikzpicture}[scale=1.0]
    \draw[->] (-2.5,0) -- (2.5,0) node[right] {$x$};
    \draw[->] (0,-2.5) -- (0,2.5) node[above] {$y$};
    \node[below left] at (0,0) {$O$};
    \draw[domain=-1.5:1.5, smooth, variable=\x, blue] plot ({\x}, {\x*\x}) node[right] {$y=x^2$};
\end{tikzpicture}
\end{document}
```
````

## 2. 函数图像绘制模式（实战经验）

以"基本初等函数图像"笔记为蓝本的标准套路：

### 2.1 坐标轴三件套

每张函数图先画：

```latex
\draw[->] (-2.5,0) -- (2.5,0) node[right] {$x$};   % x 轴，范围按函数调整
\draw[->] (0,-2.5) -- (0,2.5) node[above] {$y$};   % y 轴
\node[below left] at (0,0) {$O$};                   % 原点标记
```

坐标轴范围要比函数定义域略宽，给曲线和标注留空间。

### 2.2 函数曲线 plot

```latex
\draw[domain=起点:终点, smooth, variable=\x, 颜色] plot ({\x}, {表达式});
```

- **三角函数必须加 `r`** 将弧度转为 TikZ 的角度制：`{sin(\x r)}`、`{cos(\x r)}`、`{tan(\x r)}`。漏掉 `r` 是渲染结果错误的最常见原因。
- **反三角函数要换算**：`asin`/`acos`/`atan` 返回角度值，需 `/180*pi` 转回弧度，如 `{asin(\x)/180*pi}`。
- 常用表达式：`sqrt(\x)`、`exp(\x)`、`ln(\x)/ln(2)`（换底）、`1/\x`、`\x*\x`。
- 颜色可用 `blue`、`red`、`green!60!black`、`orange` 等 xcolor 语法。

### 2.3 间断函数分段绘制

`y=1/x`、`y=tan x` 等在间断点处必须**分多段 domain 各画一条**，否则曲线会穿越渐近线连成错误直线：

```latex
% y = 1/x：分正负两段
\draw[domain=0.4:2.2, smooth, variable=\x, orange] plot ({\x}, {1/\x});
\draw[domain=-2.2:-0.4, smooth, variable=\x, orange] plot ({\x}, {1/\x});

% y = tan x：每个周期一段，避开 ±π/2
\draw[domain=-1.2:1.2, smooth, variable=\x, blue] plot ({\x}, {tan(\x r)});
\draw[domain=1.9:4.4, smooth, variable=\x, blue] plot ({\x}, {tan(\x r)});
```

### 2.4 渐近线与辅助虚线

```latex
\draw[dashed] (-2.5,1) -- (2.5,1) node[right] {$y=1$};      % 水平渐近线
\draw[dashed] (1,-2.5) -- (1,2.5) node[above] {$x=1$};      % 垂直渐近线
\draw[dashed] (1,0) -- (1,1) -- (0,1);                       % 点到轴的投影虚线
```

### 2.5 关键点标记

```latex
\filldraw (1,1) circle (1.5pt) node[above right] {$(1,1)$};
\node[below] at (1.57,0) {$\frac{\pi}{2}$};   % 轴上刻度标签
```

### 2.6 比例与排版

- `\begin{tikzpicture}[scale=1.2]` 整体缩放；tan/cot 等纵向变化剧烈的图建议 `scale=0.8`。
- 一张图只表达一个核心知识点；多函数对比图（如幂函数 y=x、y=x²、y=√x、y=1/x 同图）用不同颜色区分并各自 `node[right]` 标注。

## 3. 其他图形速查

### 3.1 pgfplots 三维/精密绘图

````
```tikz
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document}
\begin{tikzpicture}
\begin{axis}[colormap/viridis]
\addplot3[surf, samples=18, domain=-3:3] {exp(-x^2-y^2)*x};
\end{axis}
\end{tikzpicture}
\end{document}
```
````

### 3.2 circuitikz 电路图

````
```tikz
\usepackage{circuitikz}
\begin{document}
\begin{circuitikz}[american, voltage shift=0.5]
\draw (0,0) to[isource, l=$I_0$, v=$V_0$] (0,3)
  to[short, -*, i=$I_0$] (2,3)
  to[R=$R_1$, i>_=$i_1$] (2,0) -- (0,0);
\end{circuitikz}
\end{document}
```
````

### 3.3 tikz-cd 交换图

````
```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
  K & X \times_Z Y \arrow[r, "p"] \arrow[d, "q"] & X \arrow[d, "f"] \\
    & Y \arrow[r, "g"] & Z
\end{tikzcd}
\end{document}
```
````

### 3.4 chemfig 化学结构式

````
```tikz
\usepackage{chemfig}
\begin{document}
\chemfig{H_3C-CH(-[2]OH)-COOH}
\end{document}
```
````

## 4. 常见错误（务必避免）

- ❌ 忘记 `\begin{document}` / `\end{document}` → 整块不渲染
- ❌ `\usepackage` 写在 `\begin{document}` 之后 → 报错
- ❌ 加载插件不支持的宏包（如 `geometry`、`babel`、`ctex`）→ 渲染失败
- ❌ 三角函数漏写 `r`（`sin(\x)` 而非 `sin(\x r)`）→ 曲线形状完全错误
- ❌ 反三角函数忘记 `/180*pi` → 值域错误
- ❌ 间断函数只用一个 domain → 曲线穿越渐近线
- ❌ 在 tikz 代码块中写中文正文（插件对中文支持差）→ 标注统一用数学符号/英文，中文说明写在代码块外的正文里
- ❌ 用 TikZ 画纯公式 → 公式应使用 Obsidian 原生 `$$...$$`，TikZ 只画"图"

## 5. 与闪卡的配合

- 数学笔记中 tikz 图属于"图像资料"，**不要把 tikz 代码塞进闪卡答案**；闪卡只考概念与公式文本，图像留在正文标题下供对照。
- 函数性质类闪卡示例：`指数函数 y=a^x（a>1）的图像恒过定点::(0,1)。`
