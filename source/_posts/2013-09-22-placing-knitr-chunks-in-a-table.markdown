---
layout: post
title: "'Placing knitr chunks in a table'"
date: 2013-09-22 10:42
comments: true
categories: stackoverflow, knitr, statwonk 
---

[knitr](yihui.name/knitr/) is as the tagline says, 'elegant, flexible and fast dynamic report generation with R'.

Basically, knitr allows you to embed R in LaTeX documents (and HTML documents, too!).  It gives you a large amount of control over how the R output appears in the document.

The following is a [Stackoverflow](http://stackoveflow.com) question
that I answered: [placing knitr chunks including plots inside a LaTeX layout (table)](http://stackoverflow.com/questions/18936330/placing-knitr-chunks-including-plots-inside-a-latex-layout-table/18937206#18937206).

The author was having difficulties with this code snippet:

{% codeblock lang:latex %}
\documentclass{article}
\usepackage{float}

\begin{document}
  \begin{table}
    \begin{tabular}{ll}
    A & 
    <<results1>>=
    plot(1,1)
    @ \\
    B & 
    <<results2>>=
    table(rnorm(10))
    @
    \end{tabular}
  \end{table}
\end{document}
{% endcodeblock %}

My solution was to move `\\` from line 10 to its own line, change the first chunk's option `echo=FALSE` to false. I also put the second bit of R code in an inline block using the `\Sexpr{}` command and appended a new line break `\\`.

{% codeblock lang:latex %}
\documentclass{article}
\usepackage{float}

\begin{document}
  \begin{table}
    \begin{tabular}{ll}
    A & 
    <<results1, echo=FALSE>>=
    plot(1,1)
    @ 
    \\
    B & \Sexpr{table(rnorm(10))}
    \\
    \end{tabular}
  \end{table}
\end{document}
{% endcodeblock %}

Enjoyed playing with this little chunk of knitr!
