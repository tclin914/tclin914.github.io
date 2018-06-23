---
title: BibTex轉成bibitem格式
categories: Latex
tags: Latex
abbrlink: 232e580e
date: 2017-02-05 21:35:15
updated: 2017-02-05 21:35:15
---

在撰寫碩士論文中，遇到需要把BibTex轉成bibitem格式的情況，在此紀錄下方法

建立一個dummy.tex的檔案

將以下內容複製貼上

```tex
    \documentclass{article}
    \begin{document}
    \nocite{*}
    \bibliography{reference}
    \bibliographystyle{plain}
    \end{document}
```

接著在Termimal輸入

```
    $ latex dummy
    $ bibtex dummy
    $ bibtex dymmy
    $ latex dummy
```

dummy.bbl裡頭便是\bibtem格式了
