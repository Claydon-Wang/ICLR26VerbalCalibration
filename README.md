# CalibSFT paper

论文源码与图片仓库。主文件为 `CalibSFT.tex`，`CalibSFT.pdf` 为提交时编译的版本。

## 获取与编译

```bash
git clone git@github.com:Claydon-Wang/ICLR26VerbalCalibration.git
cd ICLR26VerbalCalibration
latexmk -pdf -interaction=nonstopmode -halt-on-error CalibSFT.tex
```

编译需要包含常用 LaTeX 宏包的 TeX Live 和 `latexmk`。

## 同步修改

开始编辑前，在工作区干净时拉取最新版本：

```bash
git pull --ff-only
```

编辑后编译并提交：

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error CalibSFT.tex
git add -A
git commit -m "Update manuscript"
git push
```

仓库包含 LaTeX 源码、`reference.bib`、模板文件、`figures/` 及论文 PDF。实验记录统一放在上级目录的 [`aaa_research/iclr/exp/`](../../exp/)。编译临时文件由 `.gitignore` 排除。
