---
title: Basic frame of LATEX
author: ''
date: '2019-03-20'
slug: basic-frame-of-latex
categories: []
tags:
  - Study
description: ''
externalLink: ''
series: []
---
&emsp;每天上来看一看，以防忘记LATEX...
## 导言区
* 在这里添加各种宏包，页面控制，全局默认字体等等要素：

    命令为 `\usepackage{}`。

* 在这里添加在‘正文’中经常使用的‘命令’或‘环境’，也就是LATEX默认命令的调用的格式更改：

  * 如建立一个新的角度命令：`\newcommand\degree{^\circ}` ，这样就能用 `\degree` 来表示 `^\circ`了。

  * 如建立一个新的引用环境：
      
      ```
      \newenviroment{myquote} 
      {\begin{quote}\kaishu\zihao{5}} 
      {\end{quote}}
      ```                
      
      &emsp;这样就建立了一个 `myquote` 的新环境，可以使用 `quote` 的同时还对字体等格式进行修改。

* 在这里对封面进行编辑：

  * 标题使用 `\title{}`
  * 作者使用 `\author{}`
  * 日期使用 `\date{}` 等等。

## 正文区
&emsp;这里所指的正文是除了封面以外所有的页面，其中包括目录、摘要、正文和参考文献等页面。这些页面包括在

```
\begin{documnet}
  
\end{documnet}
```  

之中。

&emsp;相关命令为：

* 摘要： 

    ```
\begin{abstract}

\end{abstract}
    ```
    
* 目录：

    ```
    \tableofcontents
    (调用正文中的章节形成目录)
    ```

* 章节：

    ```
    \chapter{}
        contents;
    \section{}
        contents;
    (可以说这里就开始是正文)
    ```

* 插图：

    &emsp;需要在导言区添加宏包 `\usepackage{graphicx}` 和 `\usepackage{float}` 以使得能够插入图片和浮动图片。

    ```
    \begin{figure}[ht]
    \centering
    \includegraphics[scale=0.6]{figureyouwanttoinsert.jpg}
    \caption{the title of the figure}
    \label{the label of the figure}
    \end{figure}
    ```

* 表格
      
    &emsp;表格需要添加宏包 `\usepackage{multirow}`。但表格手动输入代码太麻烦了，不过主体内容可以在[网站](http://www.tablesgenerator.com/)上编辑好后复制粘贴。

    ```
    \begin{table}[H]
    \caption{the title of the table}
    \centering
    \label{the label of the table}
    \begin{tabular}
      the content of the table;
    \end{tabular}
    \end{table}
    ```
    
* 公式

    &emsp;公式编辑是LATEX的一大特色，不过比较复杂，这里添加一些常用的命令。需要在导言区添加宏包 `\usepackage{amsmath}` 和 `\usepackage{amssymb}`。

    ```
    \begin{equation}
    \label{the label of the equation}
    \end{equation}
    ```
    
    &emsp;这里说明一些命令：
      
    * 单个方程： 在 `{}` 填写 `equation` 表公式后带上编号； `equation*` 表不带上编号。
      
    * 多个方程：这其中由两种方式，一种是每一条公式居中显示，一种是每条公式按照特定的字符对齐显示，如头字符或者‘=’，‘+’号。
      
      * 每条公式居中显示：使用 `gather` 和 `gather*`。
        
     * 每条公式对齐显示：使用 `align` 和 `align*`。
        
     * 不过这里需要留意，多行公式默认每个公式进行编号，举个例子：
        
          ```
          \begin{align}
              a + b &= c \notag 
              d + e &= f \\
              g + h &= i 
          \end{align}
          ```
        
          &emsp;上面的 `&`是关系符，各公式按照 `=` 对齐； `\notag` 表示编译时候不添加编号，即第一条公式无编号，后面两条公式有编号； `\\` 表示换行，上面的第一条公式与第二条公式是在同一行的，第三条公式在第二行。
          
* 参考文献

    &emsp;在导言区添加 `\bibliographystyle{}` 表示参考文献的格式。在正文末尾添加 `\bibliography{conference}` ，表示调用 `conference.bib` 文件。
      
    &emsp;在需要添加引用的地方后使用命令 `\cite{bibtexkey}`，当需要引用多个文献时可 `\cite{bibtexkey1,bibtexkey2}`。
      
* 一些常用的命令
      
    * `\ref{labelname}` : 这个命令可以用来引用‘编号’，如‘第一章’的‘一’；前面的图和表格都有 `\label{}` 命令，`\label{}`也可以为‘第一章’进行标签：
      
        ```
        \chapter{firstchaptername}
        \label{fircha}
        这是第\ref{fircha}章。
        ```
      
        &emsp;上面显示的结果是 `这是第一章`，其实就是交叉引用。
      
    * `\eqref{eqlabelname}`，是特定引用公式的标号，引用后会显示 `式（1）`；若使用 `\ref{}` 则会显示 `式1`。
      
To be continue....