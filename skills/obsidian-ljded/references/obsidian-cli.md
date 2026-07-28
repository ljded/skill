# obsidian-cli 自动化指南

本文档是用 obsidian-cli 管理 Obsidian 仓库的**权威依据**。核心认知：**vault 就是磁盘上的普通文件夹**。

## 0. Vault 结构

- 笔记：`*.md`（纯文本 Markdown，任何编辑器都能改）
- 配置：`.obsidian/`（工作区与插件设置——**脚本不要碰这个目录**）
- 画布：`*.canvas`（JSON）
- 附件：Obsidian 设置中指定的文件夹（图片/PDF 等）

## 1. 定位 vault（先做这步，不要猜路径）

Obsidian 桌面端在配置文件中登记所有 vault（权威来源）：

- **Windows**: `%APPDATA%\obsidian\obsidian.json`（即 `C:\Users\<用户名>\AppData\Roaming\obsidian\obsidian.json`）
- **macOS**: `~/Library/Application Support/obsidian/obsidian.json`

读取该 JSON，取 `"open": true` 的 vault 条目；vault 名通常就是**文件夹名**。

已设默认 vault 时的快捷方式：

```bash
obsidian-cli print-default --path-only
```

注意：
- 多 vault 很常见（工作/个人、不同同步盘）。**不要猜，读配置**。
- 不要把 vault 绝对路径硬编码进脚本；优先读配置或用 `print-default`。

## 2. 命令速查

### 设默认 vault（一次性）

```bash
obsidian-cli set-default "<vault-文件夹名>"
obsidian-cli print-default            # 查看当前默认
obsidian-cli print-default --path-only # 只输出路径
```

### 搜索

```bash
obsidian-cli search "关键词"          # 按笔记名搜索
obsidian-cli search-content "关键词"  # 全文搜索（显示片段+行号）
```

### 创建

```bash
obsidian-cli create "文件夹/新笔记" --content "内容" --open
```

- 依赖 Obsidian URI 处理器（`obsidian://…`）可用（即已安装 Obsidian）。
- 避免在隐藏点文件夹（如 `.something/...`）下用 URI 建笔记，Obsidian 可能拒绝。

### 移动/重命名（安全重构）

```bash
obsidian-cli move "旧路径/笔记" "新路径/笔记"
```

- **会自动更新全库的 `[[wikilinks]]` 和常见 Markdown 链接**——这是相对系统 `mv` 的核心优势。
- 移动/重命名笔记**必须**用此命令，禁止用 `mv`。

### 删除

```bash
obsidian-cli delete "路径/笔记"
```

## 3. 决策原则：直接编辑 vs CLI

| 场景 | 方式 |
|------|------|
| 修改笔记内容（改文字、加闪卡、插图代码块） | **直接编辑 `.md` 文件**，Obsidian 自动感知，无需 CLI |
| 移动/重命名笔记 | **必须用 `obsidian-cli move`**（同步双链） |
| 删除笔记 | `obsidian-cli delete`（或按用户习惯走系统回收站） |
| 批量搜索定位 | `obsidian-cli search-content` |
| 新建大量笔记/自动化流水线 | `obsidian-cli create`，或直接写文件后让 Obsidian 自行索引 |

原则：**CLI 用于"仓库级"操作（移动、删除、搜索、批量创建），内容级修改直接动文件**。

## 4. 注意事项

- Windows 下路径分隔符在 CLI 参数中用 `/` 或转义 `\`；Git Bash 环境注意引号包裹含空格与中文的路径。
- 对笔记做任何批量修改前，先确认 vault 路径正确，避免写错仓库。
- obsidian-cli 需要另行安装：macOS 可用 `brew install yakitrak/yakitrak/obsidian-cli`；Windows/Linux 从 GitHub Releases（github.com/yakitrak/obsidian-cli）下载二进制，或用 `go install` 安装。未安装时退化为直接读写 vault 文件夹，并告知用户。
