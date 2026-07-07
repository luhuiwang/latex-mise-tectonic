# 中文 LaTeX 极简模板 (基于 mise + Tectonic)

基于 [mise](https://mise.jdx.dev) 包管理器 + [Tectonic](https://tectonic-typesetting.github.io) 引擎的中文 LaTeX 写作环境，无需安装 TeX Live/MiKTeX，即开即用。

## 特性

- **零 TeX Live 依赖** — Tectonic 自动下载缺失宏包
- **mise 一键安装** — `mise install` 自动安装 Tectonic、Biber、chktex、tex-fmt
- **自动参考文献处理** — `mise run buildx` 一次命令完成编译全流程
- **VS Code 深度集成** — 预配 LaTeX Workshop（编译/格式化/代码检查）
- **容器开发支持** — 提供 `.devcontainer/` 配置（Docker / WSLC）
- **中文开箱** — 使用 `ctexart` 文档类，Noto CJK 字体

## 前置要求

- [mise](https://mise.jdx.dev/getting-started.html)（跨平台包管理器）
- 或 [Docker](https://www.docker.com)（使用容器开发）

## 快速开始

```bash
# 1. 安装工具（自动从 mise.toml 读取版本）
mise install

# 2. 编译生成 PDF
mise run build

# 3. 打开 main.pdf 查看结果
```

## 使用方式

### 编译命令

| 命令 | 说明 |
|------|------|
| `mise run build` | 单次编译（Tectonic 自动下载宏包） |
| `mise run buildx` | 分步编译：tectonic → biber → tectonic ×2（完整参考文献处理） |
| `mise run clean` | 清理 `.aux` `.bbl` `.blg` 等中间文件 |
| `mise run rebuild` | 清理后重新编译 |

### VS Code 集成

项目已预配 `.vscode/settings.json`，LaTeX Workshop 扩展可直接使用：

- **编译**：绑定 `mise run build`（支持 `Ctrl+Alt+B`）
- **格式化**：使用 `tex-fmt`（支持 `Alt+Shift+F`）
- **代码检查**：使用 `chktex`，实时 lint

推荐的 VS Code 扩展见 `.vscode/extensions.json`，打开项目时会自动提示安装。

## 项目结构

```
.
├── main.tex              ← 主文档（ctexart + biblatex）
├── references.bib        ← 参考文献库（BibTeX 格式）
├── main.pdf              ← 编译输出
├── mise.toml             ← mise 工具/任务/环境配置
├── .devcontainer/
│   ├── devcontainer.json ← 容器开发配置
│   └── Dockerfile        ← 容器镜像定义
└── .vscode/
    ├── extensions.json   ← 推荐扩展
    └── settings.json     ← LaTeX Workshop 预配置
```

## 工具清单

| 工具 | mise 后端 | 版本 | 用途 |
|------|-----------|------|------|
| tectonic | `conda` | 0.16.9 | LaTeX 编译引擎（Rust 编写） |
| biber | `http` | 2.17 | 参考文献处理 |
| chktex | `conda` | 1.7.9 | LaTeX 代码检查（lint） |
| tex-fmt | `conda` | 0.5.7 | LaTeX 代码格式化 |

`mise.toml` 中通过 `[env]` 自动将 biber 路径加入 `PATH`，无需手动配置。

## 容器开发环境

### Docker

```bash
docker build -t latex-mise-tectonic .
docker run --rm -v "$PWD:/workspace" latex-mise-tectonic mise run build
```

### VS Code Dev Containers

打开项目 → 左下角绿色图标 → "Reopen in Container"。

容器会自动：
- 安装 mise 及所有工具
- 挂载 volume 缓存 mise 数据和 Tectonic 缓存
- 配置中文 locale（`zh_CN.UTF-8`）和 Noto CJK 字体
- 安装 LaTeX Workshop、mise 和 OpenChamber 扩展

### WSLC（Windows Subsystem for Linux Container）

Windows 用户可使用 WSLC 预览版替代 Docker Desktop：

```powershell
wsl --update --pre-release
```

在 VS Code 设置中将 Docker Path 改为 `wslc`，然后按 Dev Containers 流程打开即可。

## 自定义

### 添加参考文献

编辑 `references.bib`，按 BibTeX 格式添加条目，在 `main.tex` 中用 `\cite{key}` 引用。

### 修改编译命令

编辑 `mise.toml` 中的 `[tasks.*]` 区块自定义编译流程。

### 添加工具

在 `mise.toml` 的 `[tools]` 中添加，例如：
```toml
"conda:latexindent" = "latest"
```
