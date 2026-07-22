# LaTeX 常用语法教程

## 项目用途

这是一个可以直接编译的中文 LaTeX 入门项目。它适合英语基础较弱的初学者：先读中文说明，再把左侧的 `.tex` 源码和右侧的 PDF 效果对照起来。八个编号章节依次讲解安装与编译、文档结构、文字排版、表格与图片、数学公式、定理与参考文献、大型项目和 Beamer 演示文稿。

项目有两个独立的“根文件”。根文件就是一次编译的总入口，它负责载入需要的内容并生成自己的 PDF。

## 两个编译入口

| 根文件 | 生成结果 | 负责的内容 |
| --- | --- | --- |
| `main.tex` | `build/main.pdf` | 文章教程，按顺序载入 `sections/01-...tex` 到 `sections/08-...tex` |
| `beamer-demo.tex` | `build/beamer-demo.pdf` | 独立的 Beamer 幻灯片示例，不由 `main.tex` 载入 |

编辑任意编号章节时会构建 `main.tex`，因为每个编号章节开头都用 `% !TEX root = ../main.tex` 声明了根文件。不要单独编译编号章节。

编辑 `beamer-demo.tex` 时，它会构建自己的 `build/beamer-demo.pdf`。它与 `main.tex` 是并列的第二个入口，不会生成或改写 `build/main.pdf`。

## 文件夹地图

下面只列出需要阅读、编辑或保留的项目文件；编译产生的辅助文件不列在树中。

```text
latex-tutorial/
├── .vscode/
│   └── settings.json                 # 本项目的保存、编译、PDF 和 SyncTeX 设置
├── build/                             # 两个根文件的 PDF、SyncTeX 和辅助文件
├── figures/
│   ├── example-figure.tex            # 示例图片的 LaTeX 源码
│   └── example-figure.pdf            # 文章和幻灯片共同使用的图片
├── sections/
│   ├── 01-getting-started.tex        # 第 1 章：开始使用
│   ├── 02-document-structure.tex     # 第 2 章：文档结构
│   ├── 03-text-formatting.tex        # 第 3 章：文字排版
│   ├── 04-tables-and-figures.tex     # 第 4 章：表格与图片
│   ├── 05-mathematics.tex            # 第 5 章：数学公式
│   ├── 06-theorems-and-references.tex # 第 6 章：定理、引用与参考文献
│   ├── 07-large-projects.tex         # 第 7 章：大型项目
│   └── 08-beamer.tex                 # 第 8 章：Beamer 入门
├── main.tex                          # 文章教程根文件
├── beamer-demo.tex                   # Beamer 演示根文件
├── references.bib                    # 参考文献数据库
└── README.md                         # 当前说明
```

## 建议学习顺序

1. 先打开 `main.tex`，找到导言区、正文开始处和八条 `\input{...}`，理解它如何组织整篇文章。
2. 依次阅读 `sections/01-getting-started.tex` 到 `sections/06-theorems-and-references.tex`；每改一小处，就保存并对照 `build/main.pdf`。
3. 阅读 `sections/07-large-projects.tex`，理解为什么要把大文档拆成多个文件，以及根文件声明的作用。
4. 阅读 `sections/08-beamer.tex`，了解文章和幻灯片的区别。
5. 最后单独打开 `beamer-demo.tex`，编译并对照 `build/beamer-demo.pdf`，观察 frame、主题、公式、图片和逐项显示效果。

刚开始时一次只改一个地方。若编译失败，先撤回刚才的修改，再按“固定排错顺序”检查。

## VS Code 常用操作

先安装 VS Code 扩展 **LaTeX Workshop**，再用 VS Code 打开整个 `latex-tutorial` 文件夹。项目会在停止输入约 1 秒后自动保存，并在保存时自动构建；默认使用第一条 XeLaTeX recipe。打开编号章节时，扩展会按文件首行的根文件声明构建 `main.tex`。

- Command+Option+B: 手动编译当前根文件。
- Command+Option+V: 在右侧打开 PDF。
- Command+Option+J: 从源码定位到 PDF。
- 双击 PDF: 返回对应源码。
- Command+Option+C: 清理辅助文件。

“从源码定位到 PDF”和“双击 PDF 返回源码”合称 SyncTeX（源码与 PDF 同步定位）。如果 VS Code 显示受限模式，只应在确认项目来源可信后信任文件夹，否则自动构建和快捷键可能无法正常工作。

## 终端编译

在终端进入 `latex-tutorial` 文件夹后，按需要原样运行下面对应的一条命令。第一条完整构建文章，第二条完整构建 Beamer；不要把编号章节文件名放在命令末尾。

```bash
/Library/TeX/texbin/latexmk -xelatex -synctex=1 -interaction=nonstopmode -file-line-error -outdir=build main.tex
/Library/TeX/texbin/latexmk -xelatex -synctex=1 -interaction=nonstopmode -file-line-error -outdir=build beamer-demo.tex
```

命令中的 `latexmk` 会根据需要重复运行 XeLaTeX，并为文章自动处理参考文献。`-outdir=build` 把 PDF、SyncTeX 和辅助文件统一放入项目根目录的 `build`；`-synctex=1` 生成同步定位信息，`-interaction=nonstopmode` 让编译在发现问题后继续记录完整日志，`-file-line-error` 让错误显示源文件和行号。

## 常见辅助文件

编译后除了 PDF，还会在 `build` 中看到一些由工具自动生成的文件。它们不是正文源码，丢失后可以重新编译生成。

| 文件或后缀 | 用途 |
| --- | --- |
| `.aux`、`.toc`、`.out` | 保存交叉引用、目录和书签信息 |
| `.bbl`、`.bcf`、`.blg`、`.run.xml` | 保存参考文献处理过程和结果 |
| `.fdb_latexmk`、`.fls` | 帮助 `latexmk` 判断依赖和是否需要重编译 |
| `.log` | 编译日志；排错时先看这里 |
| `.synctex.gz` | 支持源码与 PDF 双向定位 |
| `.nav`、`.snm`、`.vrb` | Beamer 的导航、页面和逐字代码辅助信息 |
| `.xdv` | XeLaTeX 生成 PDF 过程中的中间文件 |

`build/main.pdf`、`build/beamer-demo.pdf` 和 `figures/example-figure.pdf` 是要查看或使用的结果，不属于应随手删除的辅助文件。

## 固定排错顺序

1. 在 VS Code 的 Problems 面板或对应 `.log` 中找到第一条真正的错误；后面的错误常常只是连锁反应。
2. 根据错误给出的文件名和行号，检查刚修改的位置，特别是花括号、数学模式符号，以及 `\begin{...}` 和 `\end{...}` 是否成对。
3. 检查文件名和相对路径是否完全一致，包括 `sections/`、`figures/` 和 `references.bib`。
4. 确认入口正确：编号章节应指向 `main.tex`；Beamer 应直接编译 `beamer-demo.tex`。
5. 如果看到引用未解析，先确认引用键存在，再完整编译；参考文献通常需要多轮处理。
6. 清理辅助文件，然后重新运行“终端编译”中的对应完整命令。
7. 仍然失败时，只保留最小修改逐步尝试，并把日志中的第一条错误连同源文件行号一起检查。

## 清理辅助文件

在 VS Code 中打开当前根文件，然后使用下面的快捷键：

- Command+Option+C: 清理辅助文件。

LaTeX Workshop 会对当前根文件在 `build` 中执行标准清理（相当于 `latexmk -outdir=build -c`）。这种“小清理”可能保留 `.bbl`、`.synctex.gz` 等可继续复用的文件；看到它们仍在是正常现象。标准清理不会删除 `.tex`、`.bib`、项目图片等源码，也不会删除最终 PDF。

两个根需要分别清理：清理 `main.tex` 后，如还要清理 Beamer 的辅助文件，请再打开 `beamer-demo.tex` 执行一次。清理不会替代编译；需要 PDF 时，重新保存或运行对应的完整构建命令即可。
