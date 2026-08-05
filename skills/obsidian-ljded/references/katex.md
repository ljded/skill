# KaTeX / LaTeX 数学公式指南

本文档是在 Obsidian 笔记中编写数学公式的**权威语法依据**。写任何 `$...$` / `$$...$$` 公式前可对照速查。

## 0. Obsidian 数学渲染基础

- 行内公式：`$...$`；独立成段公式：`$$...$$`（独占段落、居中显示）。
- Obsidian 使用 MathJax 引擎渲染，语法与 KaTeX 基本兼容，常见 LaTeX 数学命令均可使用。
- 职责划分：**纯公式用 `$`/`$$`；函数图像、几何图等"图"用 TikZ**（见 `references/tikzjax.md`），不要混用。
- 换行：`\\` 仅在 `align`/`gathered`/`cases`/`matrix` 等 `\begin...\end` 环境**内部**有效；`$$` 顶层裸 `\\` 在 Obsidian 中不生效，多个独立公式应拆成多个 `$$` 块（详见第 13 节避坑清单）。

## 1. 希腊字母

规则：**英文全拼即命令**；首字母大写得大写；加 `var` 前缀为变体（variant）。

| 字母 | 命令 | 变体 | 大写 | 大写变体 |
|------|------|------|------|---------|
| $\alpha$ | `\alpha` | | | |
| $\beta$ | `\beta` | | | |
| $\gamma$ | `\gamma` | | `\Gamma` | `\varGamma` |
| $\delta$ | `\delta` | | `\Delta` | `\varDelta` |
| $\epsilon$ | `\epsilon` | `\varepsilon` | | |
| $\zeta$ | `\zeta` | | | |
| $\eta$ | `\eta` | | | |
| $\theta$ | `\theta` | `\vartheta` | `\Theta` | `\varTheta` |
| $\iota$ | `\iota` | | | |
| $\kappa$ | `\kappa` | | | |
| $\lambda$ | `\lambda` | | `\Lambda` | `\varLambda` |
| $\mu$ | `\mu` | | | |
| $\nu$ | `\nu` | | | |
| $\xi$ | `\xi` | | `\Xi` | `\varXi` |
| $\pi$ | `\pi` | `\varpi` | `\Pi` | `\varPi` |
| $\rho$ | `\rho` | `\varrho` | | |
| $\sigma$ | `\sigma` | `\varsigma` | `\Sigma` | `\varSigma` |
| $\tau$ | `\tau` | | | |
| $\upsilon$ | `\upsilon` | | `\Upsilon` | `\varUpsilon` |
| $\phi$ | `\phi` | `\varphi` | `\Phi` | `\varPhi` |
| $\chi$ | `\chi` | | | |
| $\psi$ | `\psi` | | `\Psi` | `\varPsi` |
| $\omega$ | `\omega` | | `\Omega` | `\varOmega` |

注意：小写 o 就是普通字母 `o`（无斜线），无专门命令。

## 2. 上下标

- `^` 上标，`_` 下标：`a^2`、`a_1`、`a^2_1`
- **多项内容必须套 `{}`**：`a^{a+b}`（写成 `a^a+b` 只有第一个字符进上标）

## 3. 斜体与直立体（排版规范）

规范：**只有变量或单字符函数名用斜体，其余（多字母函数名、单位、文字）用直立体**。

转直立体两个命令：
- `\rm`（roman）：不显示空格；不套 `{}` 时对后续全部内容有效
- `\text{}`：可显示空格与中文；不套 `{}` 时只对后一个字符有效

示例：`\text d x`（微分 d 用直立体）、`\text{其他}`。

## 4. 分式与根式

- `\frac{分子}{分母}`（fraction）；`\dfrac` 为 display-style（行内也显示为大分式）
- `\sqrt{a}` 平方根（square root）；`\sqrt[n]{a}` n 次根

## 5. 普通运算符

| 命令 | 含义 | 助记 |
|------|------|------|
| `\times` | 乘 × | |
| `\cdot` | 点乘 · | centre |
| `\div` | 除 ÷ | divide |
| `\pm` / `\mp` | 正负 / 负正 | plus-minus / minus-plus |
| `\ge` / `\le` | ≥ / ≤ | greater/less than or equal |
| `\gg` / `\ll` | 远大于 / 远小于 | |
| `\ne` | ≠ | not equal |
| `\approx` | ≈ | approximate |
| `\equiv` | ≡ 恒等于 | equivalent |
| `\cap` `\cup` | 交 ∩ 并 ∪ | |
| `\in` `\notin` | 属于 / 不属于 | |
| `\subseteq` `\subsetneqq` | 子集 / 真子集（不等） | |
| `\varnothing` | 空集 | |
| `\forall` `\exists` `\nexists` | 任意 / 存在 / 不存在 | |
| `\because` `\therefore` | 因为 ∵ / 所以 ∴ | |
| `\mathbb R` | 黑板粗体（实数集 ℝ） | blackboard bold |
| `\mathcal F` | 书法体 | calligraphy |
| `\mathscr F` | 手写体 | script |
| `\cdots` `\vdots` `\ddots` | 水平/垂直/对角省略号 | vertical / diagonal |
| `\infty` | 无穷 ∞ | infinity |

极限写法：`\lim_{x\to0}\frac{x}{\sin x}`（下标写趋向）。

## 6. 大型运算符与间距

- `\sum` 求和、`\prod` 乘积（product）、`\int` 积分（integral）、`\iint`、`\iiint`、`\oint`（环路积分）
- 上下限位置：行内默认写在角标位（`\sum_i^n`）；用 `\limits` 强制写到正上下方（`\sum\limits_{i=1}`）
- **微分前加小间距**：`f(x)\,\text d x`

间距命令（从小到大）：
- `\,` 小间距
- `\ ` 普通空格
- `\quad` 大空格
- `\qquad` 特大空格

## 7. 标注符号（字母上方）

- `\vec{a}` 向量（vector，短箭头）；`\overrightarrow{AB}` 长箭头（多字符用它）
- `\bar{x}` 短横（平均值）；`\overline{AB}` 长横（多字符用它）

完整清单：`\hat` `\bar` `\acute` `\check` `\grave` `\vec` `\tilde` `\mathring` `\dot` `\ddot`；多字符版本 `\widehat{AAA}`、`\widetilde{AAA}`。

## 8. 箭头

`\leftarrow` `\rightarrow` `\Rightarrow` `\Leftrightarrow` `\longleftarrow` `\longrightarrow` 等；`\to` 是 `\rightarrow` 的简写（极限里常用 `x\to0`）。

## 9. 括号与定界符

- 普通：`()` `[]` `\{\}`（花括号要转义）
- 特殊：`\lceil \rceil`（上取整）、`\lfloor \rfloor`（下取整）、`\|`（范数双竖线）
- **自适应大小**：`\left( ... \right)` 随内容自动撑大，如 `\left(0,\frac{1}{a}\right]`
- 只显示一侧：用 `\left.` 占位，如求值竖线 `\left.\frac{\partial f}{\partial x}\right|_{x=0}`

## 10. 多行公式（align）

```latex
\begin{align}
a &= b + c + d \\
  &= e + f
\end{align}
```

- `align` 环境对齐多行；**`&` 是对齐锚点**（通常放在 `=` 前），`\\` 换行。
- **注意**：`\\` 只有在 `align`/`gathered`/`cases`/`matrix` 这类 `\begin...\end` 环境内部才换行；`$$` 顶层的裸 `\\` 在 Obsidian 中不生效（见第 13 节避坑清单）。

## 11. 分段函数（cases）

```latex
f(x)=
\begin{cases}
\sin x, & -\pi \le x \le \pi \\
0,      & \text{其他}
\end{cases}
```

`cases` 环境自动带左大括号；每行 `&` 对齐条件，`\\` 分行；中文条件文字放 `\text{}` 里。

## 12. 矩阵

```latex
\begin{matrix}
a      & b      & \cdots & c      \\
\vdots & \vdots & \ddots & \vdots \\
e      & f      & \cdots & g
\end{matrix}
```

- 环境选择：`matrix` 无框、`bmatrix` 方括号 `[ ]`（bracket）、`pmatrix` 圆括号 `( )`（parenthesis）、`vmatrix` 竖线 `| |`（行列式，vertical bar）
- `&` 分列、`\\` 分行
- **矩阵字母加粗**：`\bf A`（bold face）

## 13. Obsidian 避坑清单

- ❌ **`$$...$$` 顶层用裸 `\\` 换行 → Obsidian 中不生效**！`\\` 只在 `\begin{...}...\end{...}` 环境（`align`、`gathered`、`cases`、`matrix` 等）**内部**有效。多个独立公式的正确做法：**拆成多个 `$$` 块**（推荐，也是公式表笔记的惯例），或整体套 `gathered` 环境：

```latex
% 正确写法 1：拆多个 $$ 块（推荐）
$$
a^2 - b^2 = (a+b)(a-b)
$$
$$
a^3 + b^3 = (a+b)(a^2 - ab + b^2)
$$

% 正确写法 2：gathered 环境内用 \\
$$
\begin{gathered}
a^2 - b^2 = (a+b)(a-b)\\
a^3 + b^3 = (a+b)(a^2 - ab + b^2)
\end{gathered}
$$
```

- ❌ 行内公式 `$` 与内容之间加空格（`$ x $`）→ 可能不渲染；写成 `$x$` 紧贴
- ❌ 上标/下标多项不套 `{}`（`x^10` 只会上标 1）→ 写 `x^{10}`
- ❌ 花括号不转义（`\{ \}` 才显示 { }）
- ❌ 表格 `| ... |` 单元格内写裸 `|`（如 `$|x|$`）→ 竖线会被当成表格分隔符；用 `$\vert x \vert$` 或在表格内改用 `\lvert`/`\rvert`
- ❌ 多字母函数名写斜体（`sin` 应为 `\sin`；`lim` 应为 `\lim`）→ 常见函数都有现成命令：`\sin` `\cos` `\tan` `\ln` `\log` `\lim` `\max` `\min` 等
- ❌ **用未定义的函数命令**（如 `\arccot`）→ 渲染失败（MathJax 只内置 `\arcsin` `\arccos` `\arctan`，**没有** `\arccot`/`\arcsec`/`\arccsc`）。正确写法：用 `\operatorname{}` 包裹 → `\operatorname{arccot} x`，或改写为 `\cot^{-1}x`。通用规则：**任何非内置的函数名都套 `\operatorname{}`**（如 `\operatorname{sech}`）
- ❌ 中文直接写在公式里不套 `\text{}` → 渲染异常或乱码；中文必须放 `\text{中文}` 内
- ❌ 用 TikZ 画纯公式 → 公式走 `$`/`$$`，TikZ 只画图

## 14. 与闪卡的配合

- 公式类闪卡直接考公式本身，单行格式示例：`求根公式::对于 $ax^2+bx+c=0$，$x_{1,2}=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$。`
- 多公式对比（如降幂公式两条）适合 `?` 多行闪卡，答案区用 `$$` 块逐条列出。
- 完型填空可挖公式的关键部分：`二倍角公式 $\cos 2x =$ ==1;;$1-2\sin^2 x==$（三种等价形式之一）。`
