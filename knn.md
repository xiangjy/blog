---
title: k邻近算法
date: 2017-08-14 12:04:05
tags: [算法, 机器学习, python]
---
<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"></script>
#### k最邻近算法
余弦相似度：通过测量两个向量的夹角的余弦值来度量它们之间的相似性。0度角的余弦值是1，而其他任何角度的余弦值都不大于1；并且其最小值是-1.从而两个向量之间的角度的余弦值确定两个向量是否大致指向相同的方向。两个向量有相同的指向时，余弦相似度的值为1；两个向量夹角为90°时，余弦相似度的值为0；两个向量指向完全相反的方向时，余弦相似度的值为-1。这结果是与向量的长度无关的，仅仅与向量的指向方向相关。余弦相似度通常用于正空间，因此给出的值为0到1之间。

$$cos{\theta}=\frac{a^2+b^2-c^2}{2ab}$$

$$cos{\theta}=\frac{x_1x_2+y_1y_2}{\sqrt{x_1^2+y_1^2}*\sqrt{x_2^2+y_2^2}}$$

$$cos{\theta}=\frac{AB}{\vert{A}\vert*\vert{B}\vert}$$
