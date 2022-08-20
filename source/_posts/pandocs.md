---

title: pandoc
date: 2021-05-27

---

### pandoc引用多个文献用分号

    pandoc -o propose-template.docx --print-default-data-file reference.docx 

    pandoc -o paper-template.docx --print-default-data-file reference.docx 

    pandoc -s 片段.md --reference-doc=propose-template.docx -o 片段.docx


    pandoc 写作笔记.md --reference-doc=/Users/wenhao/draft-template-new.docx -o 写作笔记.docx


## 生成paper

    pandoc -s paper.md --reference-doc=propose-template.docx -o paper.docx

## 生成报告PPT

    pandoc 报告ppt.md --reference-doc=propose-template.pptx -o 报告PPT.pptx --slide-level=2

    pandoc -o custom-reference.pptx --print-default-data-file reference.pptx

pandoc/模版文件/自定义模版

    --data-dir 
    $HOME/.local/share/pandoc

https://github.com/jgm/pandoc-templates




### Zotero
是一个免费易用的 Firefox 扩展与客户端软件，可以协助我们收集、管理及引用研究资源，包括期刊、书籍等各类文献和网页、图片等。与Endnote等不同的是，它既可以单独使用，也可以内嵌于 Firefox 与 Google 浏览器等环境下使用。随着互联网的发展，我们获取文献资源大都是通过浏览器，而 Zotero 与浏览器的密切结合使我们的工作更加方便。
Zotero 的最大优点是能够对在线文献数据库网页中的文献题录直接抓取。
如我们打开SD数据库中的某篇文献的网页，在安装有 Zotero 的 Firefox 的地址栏末端会出现一个“页面”标志。


### NoteExpress 
是北京爱琴海软件公司开发的一款专业级别的文献检索与管理系统，其核心功能涵盖“知识采集，管理，应用，挖掘”的知识管理的所有环节，是学术研究，知识管理的必备工具，发表论文的好帮手。

### Endnote
由Thomson Corporation下属的Thomson ResearchSoft 开发。 Thomson ResearchSoft是以学术信息市场化和开发学术软件为宗旨的子公司。Thomson Corporation总部位于美国康涅狄格州的Stanford。

### Mendeley
是一款免费的跨平台文献管理软件，同时也是一个在线的学术社交网络平台。可一键抓取网页上的文献信息添加到个人的library中。还可安装MS Word和Open Office插件，方便在文字编辑器中插入和管理参考文献。参考文献格式与Zotero一样用各种期刊格式的CLS文件。Mendeley还免费提供各2GB的文献存储和100MB的共享空间


pandoc-fignos无法转换公式 错

wps无法显示数学公式(已解决)



markdown 文件包含

https://github.com/TomBener/pandoc-templates

https://sspai.com/post/64842

https://retompi.com/


## Markdown Editor

https://marketplace.visualstudio.com/items?itemName=zaaack.markdown-editor

markdown 幻灯片
marp
https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode


