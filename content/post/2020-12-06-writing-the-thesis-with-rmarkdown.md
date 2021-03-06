---
title: 用Rmarkdown写毕业论文
author: 张桐川
date: '2021-01-16'
slug: writing-the-thesis-with-rmarkdown
categories:
  - R语言
tags:
  - rmarkdown
  - thesis
  - bookdown
  - ggplot2
  - kableExtra
  - citr
forum_id: 422024
---



博士生涯终于走到了最后一步了。这次全程用 R 和 Rmarkdown 相关的包完整写完了论文，现在总结一下个人经验和踩过的坑，希望给后来人提供参考经验，同时安利一下 Rmarkdown 这个提高生产力的工具。

## 选择 Rmarkdown 的理由

大部分大学的毕业论文对排版都着严格的排版要求，具体需要注意的细节可能有

- 图表要编号，并经常需要在文中交叉引用
- 引用文献，文中和文尾的参考文献格式需要注意
- 目录，图和表的目录
- 页眉页脚，字体字号行间距，标题副标题大小等等

个人感受而言，传统 Word 虽然易用，但所见即所得式的编辑模式会将作者很大一部分精力吸引到“排版”工作上，另外大部分论文插图较多，排版也很困难。而作为另一条路的 LaTeX 则功能强大然而语法复杂。LaTeX 将文本与排版结果分离，学习曲线陡峭。使用 LaTeX 颇有为码字而先修炼成排版工人的感觉。相比Word的上手容易精通难和 LaTeX 的复杂功能， 个人觉得Rmarkdown 易用性和功能性之间达到了较好的平衡。Rmarkdown 既可实现 LaTeX 完全相同的最终论文排版效果，同时 markdown 语法简单易上手，学习成本相较 LaTeX大为降低，直逼 Word 。另外，使用 Rmarkdown 还能生成 gitbook的电子书，相较pdf格式也有良好的在线阅读体验(见[gitbook在线电子书版](https://tcgriffith.github.io/thesisdown_demo/_book/))。

值得一提的是用 Rmarkdown 写毕业论文并不适合所有人。如果读者有有一定折腾的极客精神，懂一点 R语言就更好了。

## 用 Rmarkdown 写论文

### 准备工作

首先我们需要安装 R 和 Rstudio 以及相关的包，具体包会在下面每一小节分别介绍。除此之外还需要 git/github 对文本源文件做版本控制，一个文本编辑器(我用的 sublime text)，以及用 Zotero 作为文献库管理软件。

其次，毕业论文包含很多材料，我们将创建一个项目文件夹来管理图片，数据，论文文本，引用文献记录 (bib) 等资料。这个论文项目可以保存在本地和在线保存在 github，从而实现备份和跨电脑工作的目的。

### 论文框架

前人们创建了好多基于bookdown的毕业论文模板，并在 Github 上分享。个人推荐的是 [thesisdown](https://github.com/ismayc/thesisdown)。 它提供了基于 bookdown 的论文项目骨架，有助于快速地搭好框架。这样封面、目录、页眉页脚、引用文献等零碎的格式问题就基本搞定了。剩下的主要精力可以专注于码字上。

论文项目目录如下。其中，Rmd 文件即为章节文本，bib 和 csl 目录保存文献引用记录和引用格式(apa)，data 和 figure 目录保存数据和插图，prelims 保存 Rmd 格式的简介和前言。reedthesis.cls 和 template.tex 是论文 LaTeX 模板，针对具体排版要求可能需要调整代码。

```
.
├── 01-chap1.Rmd
├── 02-chap2.Rmd
├── 03-chap3.Rmd
├── 04-conclusion.Rmd
├── 05-appendix.Rmd
├── 99-references.Rmd
├── bib
│   └── thesis.bib
├── _book
│   ├── thesis.pdf
│   └── thesis.tex
├── _bookdown.yml
├── chemarr.sty
├── csl
│   └── apa.csl
├── data
│   └── flights.csv
├── figure
│   ├── reed.jpg
│   └── subdivision.pdf
├── index.Rmd
├── prelims
│   ├── 00-abstract.Rmd
│   └── 00--prelim.Rmd
├── reedthesis.cls
├── template.tex
└── thesisdown_demo.Rproj

```

### 作图

专业的作图是学术写作的灵魂。如何用图来简明扼要地传达信息是现代科研工作的基本功。

因为主要用R做数据分析，我选择的R绘图包是大名鼎鼎的 ggplot2。在此推荐的另一个包是[patchwork](https://patchwork.data-imaginist.com/): 它采用简单的语法实现将多个ggplot图拼接成大图。其他类似功能的包有 cowplot， gridExtra，但实际体验还是patchwork更为优雅强大。

### 制表

制表用的 kable: rmarkdown引擎knitr自带。配合的包有 [kableextra](https://cran.r-project.org/web/packages/kableExtra/vignettes/awesome_table_in_html.html)。 可以实现表的标题，脚注，按行列分组等等功能。包的说明书写得非常详细，需要功能的时候对着查即可。




### 文献引用
[Zotero](https://www.zotero.org/)， zotero是在线文献管理工具，具体工作原理是创建账号之后就拥有了在线的文献库。装了zotero浏览器插件设置好之后看到文章即可将文献一键纳入收藏。这比起曾经用过的mendeley等工具方便了不少。而在R里也有相应的包与之匹配，也就是citr. 

[citr](https://github.com/crsh/citr)包的作用是在Rstudio里直接连zotero库就能直接插文献了。全程不用手动编辑.bib库或者手动维护引用索引。添加引用的完整过程就是：看论文->浏览器中用zotero将论文添加到库->在Rstudio里将光标定位到需要插入引用的地方，用citr插件的"insert citation"连接zotero库，插入添加的文献。

值得一提的是在Rstudio 1.4预览版中，Zotero管理的文献库将与Rstudio有更好的整合。无需安装额外包和配置即可在Rstudio里插入Zotero库中的文献，详情可参考[官方文档](https://blog.rstudio.com/2020/11/09/rstudio-1-4-preview-citations/)。


最终效果展示:

![Screenshot from 2021-01-10 21-20-34](https://user-images.githubusercontent.com/19829201/104121385-8c09a780-5389-11eb-8262-46ce3aac8274.png)
![Screenshot from 2021-01-10 21-19-35](https://user-images.githubusercontent.com/19829201/104121386-8dd36b00-5389-11eb-82e8-8549bf947cc1.png)

[论文repo](https://github.com/tcgriffith/thesisdown_demo)

[gitbook在线电子书版](https://tcgriffith.github.io/thesisdown_demo/_book/)
[pdf版](https://github.com/tcgriffith/thesisdown_demo/blob/main/_book/thesis.pdf)


下面聊聊我在用Rmarkdown写毕业论文中碰到的实际问题

## 如何与word选手合作

我碰到的第一个问题是：使用Rmarkdown写论文，如何与不懂Rmarkdown的合作者进行有效协作。更核心的是word强大的追踪修改的功能无法替代。

经过沟通尝试之后我最终做法还是在Rmarkdown里写好一个章节，直接复制Rmarkdown源文本进word发给导师，改完之后传回来我再改改复制进Rmarkdown生成pdf。

## 非R语言生成图片的显示问题

插入图片的markdown语法为 `![](path/of/image)`。但是在论文中的图需要控制长宽，需要有引用标签和标题等，最合适的办法是使用rmarkdown的knitr包自带函数`include_graphics()`。这样可以用与R生成的图一样的设置来精确控制图片显示。
示例代码为：

````
```{r, out.width = "400px"}
knitr::include_graphics("path/to/image.png")
```
````

## ggplot作图时标签，标注中的特殊字符

主要是公式的上标下标，在stack overflow上有丰富的案例可以参考。主要使用的是R的`expression`函数。

例如[在图中添加斯皮尔曼相关系数的注释](https://stackoverflow.com/questions/27303019/ggplot-annotate-with-greek-symbol-and-1-apostrophe-or-2-in-between-text): 

````
df <- data.frame(x =  rnorm(10), y = rnorm(10))
temp <- expression("Spearman's"~rho == 0.34)
ggplot(df, aes(x = x, y = y)) + 
  geom_point() + 
  annotate("text", x = mean(df$x), y = mean(df$y), 
           parse = T, label = as.character(temp))
````

效果:

![EoHpH](https://user-images.githubusercontent.com/19829201/104745920-55bd9500-579a-11eb-80c4-1e76e6be6403.png)




## 后记

所谓工欲善其事必先利其器，科研工作者也需要像手艺人一样对工具仔细挑选和精研。但学校课程很少专门开课传授这方面的知识。本文介绍用Rmarkdown准备毕业论文以作为word, LaTeX以外的第三种选择，希望能对有类似需求的同路人有所帮助。

最后需要强调的是，工具只是“术”，要提高学术论文质量还是得修炼内功，安利一篇[写好英语科技论文的诀窍](https://sparks-lab.org/blog/recipe-sci-paper/)，祝诸君在学术之路上武运昌隆。
