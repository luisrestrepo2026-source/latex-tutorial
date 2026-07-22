# 常见问题与固定排错顺序

LaTeX 经常会在第一处错误之后产生很多连锁报错。排错时先处理日志中的**第一条真正错误**，不要从最后一条开始猜。

## 固定排错顺序

1. 在 VS Code Problems 面板或 `build/*.log` 中找到第一条以文件名、行号或 `!` 开头的错误。
2. 检查刚修改的位置，尤其是 `{}`、数学模式符号，以及 `\begin{...}` 和 `\end{...}` 是否成对。
3. 检查相对路径和大小写，包括 `sections/`、`figures/` 与 `references.bib`。
4. 确认入口：编号章节应由 `main.tex` 编译，Beamer 应由 `beamer-demo.tex` 编译。
5. 清理对应根文件的辅助文件，再完整编译。
6. 仍然失败时，把问题缩小为一次只保留一个修改，并记录第一条错误及其文件行号。

## 终端找不到 latexmk、xelatex 或 biber

先关闭并重新打开终端，再运行：

```bash
latexmk --version
xelatex --version
biber --version
```

如果仍找不到命令，说明 LaTeX 发行版没有安装完整，或它的可执行目录不在 PATH 中。

- macOS 的 MacTeX 通常把命令放在 `/Library/TeX/texbin/`。可临时用 `/Library/TeX/texbin/latexmk --version` 判断安装是否存在；公开编译命令仍建议使用 PATH 中的 `latexmk`。
- Windows 安装 TeX Live 或 MiKTeX 后，重新登录系统或重启 VS Code，让新的 PATH 生效。
- Linux 应确认安装了包含 XeLaTeX、中文支持、BibLaTeX/Biber 和 Beamer 的 TeX Live 组件；发行版的软件包名称可能不同。

## 缺少宏包或字体

日志常见形式包括 `File 'xxx.sty' not found` 或 `The font ... cannot be found`。

1. 先记录缺失的准确文件名或字体名。
2. 使用当前 LaTeX 发行版的包管理器安装对应组件；精简版 TeX Live/MiKTeX 往往需要补装中文、数学、BibLaTeX 或 Beamer 组件。
3. 不要为了绕过错误而随意删除源码中的宏包；这可能让后续示例失效。
4. 补装后清理辅助文件并重新编译。

本教程依赖 XeLaTeX 与 `ctex` 提供中文字体配置，不应改用 pdfLaTeX 编译。

## 参考文献没有出现

确认 `main.tex` 使用完整的 `latexmk -xelatex ...` 命令编译，而不是只运行一次 XeLaTeX。`latexmk` 会按需要调用 Biber 并再次运行 XeLaTeX。

如果引用仍显示为问号：

1. 检查 `\cite{...}` 中的键是否存在于 `references.bib`。
2. 查看 `build/main.blg` 中的第一条错误。
3. 清理 `main.tex` 的辅助文件并重新完整编译。

## 编号章节单独编译失败

`sections/` 下的文件不是完整文档，没有自己的 `\documentclass`。它们通过首行的根文件声明指向 `main.tex`：

```tex
% !TEX root = ../main.tex
```

在终端中始终编译 `main.tex`；在 VS Code 中应打开整个项目文件夹，让 LaTeX Workshop 识别根文件。

## 旧辅助文件导致异常

移动文件、修改标签或切换编译入口后，旧的 `.aux`、`.bcf`、`.toc` 等文件可能与源码不一致。对出现问题的根文件运行：

```bash
latexmk -outdir=build -c main.tex
```

把命令末尾替换为实际入口，再重新完整编译。清理不会删除 `.tex`、`.bib`、图片或最终 PDF。

## SyncTeX 无法双向定位

1. 确认编译命令包含 `-synctex=1`。
2. 确认 PDF 在 VS Code 的 LaTeX Workshop 内置预览器中打开。
3. 重新编译当前根文件，确保 `build/*.synctex.gz` 是最新生成的。
4. 编号章节应继续指向 `main.tex`，否则源码与 PDF 的对应关系可能错误。

## 请求帮助时应提供什么

请同时提供：

- 正在编译的根文件名；
- 运行的完整命令；
- 操作系统和 LaTeX 发行版；
- 日志中的第一条错误及文件行号；
- 出错前最后一次源代码修改。

这些信息通常比整份长日志更快定位问题。
