# 安装与编译

本项目使用 XeLaTeX、`ctex`、BibLaTeX/Biber 和 `latexmk`。推荐用 VS Code + LaTeX Workshop 阅读源码并对照 PDF，但也可以只使用终端编译。

## 1. 安装 LaTeX 发行版

| 系统 | 推荐发行版 | 编辑器 |
| --- | --- | --- |
| macOS | [MacTeX](https://www.tug.org/mactex/) | VS Code + LaTeX Workshop |
| Windows | [TeX Live](https://www.tug.org/texlive/) 或 [MiKTeX](https://miktex.org/) | VS Code + LaTeX Workshop |
| Linux | [TeX Live](https://www.tug.org/texlive/) | VS Code + LaTeX Workshop |

优先安装完整发行版。精简安装可能缺少 `ctex`、`biblatex`、`biber`、`booktabs`、`tcolorbox` 或 Beamer 等本教程使用的组件。

安装完成后，关闭并重新打开终端，再检查：

```bash
latexmk --version
xelatex --version
biber --version
```

三个命令都应显示版本信息。如果出现“command not found”或“不是内部或外部命令”，先阅读[常见问题](troubleshooting.md#终端找不到-latexmkxelatex-或-biber)。

## 2. 安装 VS Code 扩展

1. 安装 [Visual Studio Code](https://code.visualstudio.com/)。
2. 在扩展市场安装 [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)。
3. 用 VS Code 打开整个 `latex-tutorial` 文件夹，不要只打开某一个 `.tex` 文件。
4. 仅在确认仓库来源可信后信任工作区；项目配置会在保存时运行 `latexmk`。

项目内的 `.vscode/settings.json` 使用 PATH 中的 `latexmk`，不依赖某个操作系统的固定安装路径。

## 3. 获取项目

```bash
git clone https://github.com/Tamarisk-Du/latex-tutorial.git
cd latex-tutorial
```

## 4. 编译三个入口

完整文章教程：

```bash
latexmk -xelatex -synctex=1 -interaction=nonstopmode -file-line-error -outdir=build main.tex
```

Beamer 演示：

```bash
latexmk -xelatex -synctex=1 -interaction=nonstopmode -file-line-error -outdir=build beamer-demo.tex
```

最小中文示例：

```bash
latexmk -xelatex -synctex=1 -interaction=nonstopmode -file-line-error -outdir=build example.tex
```

编译结果分别位于：

```text
build/main.pdf
build/beamer-demo.pdf
build/example.pdf
```

`main.tex` 会自动调用 Biber 处理参考文献。不要单独编译 `sections/` 下的章节文件；这些文件由 `main.tex` 统一载入。

## 5. VS Code 常用操作

| 操作 | macOS | Windows/Linux |
| --- | --- | --- |
| 编译当前根文件 | Command+Option+B | Ctrl+Alt+B |
| 预览 PDF | Command+Option+V | Ctrl+Alt+V |
| 从源码定位到 PDF | Command+Option+J | Ctrl+Alt+J |
| 清理当前根文件 | Command+Option+C | Ctrl+Alt+C |
| 从 PDF 返回源码 | 双击 PDF | 双击 PDF |

编号章节的首行包含 `% !TEX root = ../main.tex`，所以在章节中执行编译时，LaTeX Workshop 会构建 `main.tex`。`beamer-demo.tex` 和 `example.tex` 则各自独立编译。

## 6. 清理并重新编译

需要排除旧辅助文件影响时，分别对三个根执行：

```bash
latexmk -outdir=build -c main.tex
latexmk -outdir=build -c beamer-demo.tex
latexmk -outdir=build -c example.tex
```

随后重新运行第 4 节的完整编译命令。`build/` 是本地输出目录，不纳入 Git；正式 PDF 可从项目的 [Releases](https://github.com/Tamarisk-Du/latex-tutorial/releases) 下载。
