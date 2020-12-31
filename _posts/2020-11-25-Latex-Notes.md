---
layout: post
title: Latex笔记
subtitle:
author: Liang Chen
date: 2020-11-25 18:00:00 +0800
tags: [Notes, Latex]
catalog: true
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

> Reference: [一份不太简短的 LATEX 2ε 介绍](http://www.mohu.org/info/lshort-cn.pdf)

1. 多行公式如何左对齐？

    ```
    $$
    \begin{align*}
    & i_t = \sigma (W_{xi}x_t + W_{hi}h_{t-1} + W_{ci}c_{t-1} +b_i)\\
    & f_t = \sigma (W_{xf}x_t + W_{hf}h_{t-1} + W_{cf}c_{t-1} +b_f)\\
    & \bar{c_t} = tanh(W_{xc}x_t + W_{hc}h_{t-1} + b_c)\\
    & c_t = f_t \circ c_{t-1} + i_t \circ \bar{c_t}\\
    & o_t = \sigma ( W_{xo}x_t + W_{ho}h_{t-1} + W_{co}c_{t} +b_o )\\
    & h_t = o_t \circ tanh(c_t)
    \end{align*}
    $$
    ```

