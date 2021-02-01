---
layout: post
title: EDA课程实践项目
subtitle: 64x64位乘法器
author: Liang Chen
date: 2021-02-01 18:00:00 +0800
tags: [Notes, EDA, mult]
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

Reference: [https://zhuanlan.zhihu.com/p/127164011](https://zhuanlan.zhihu.com/p/127164011)

Project Address: [https://github.com/cliang935/EDA_course_design](https://github.com/cliang935/EDA_course_design)

> 作者按：
> 1. 这是研一时一门课的课程设计，实现的功能是有符号无符号兼容的64x64位乘法器，并且对其进行了前仿真、代码覆盖率查看、DC综合、FM一致性检查、VCS后仿真等一系列操作，目的是熟悉相关EDA工具的使用，后期希望可以补充上PT的STA和布局布线的内容。
> 2. 局限性是在于后仿并不准确，其所使用的sdf文件来自于DC综合的输出，但是更准确的sdf文件应该用PT的输出。
> 3. 我的设计是直接从原理的角度实现了64位的乘法器，这就导致设计综合后的面积更大了。我的思考是在实际的项目中，应该用子乘法器去实现高位的乘法器，要么同时使用多个子乘法器，要么分时复用一个或两个子乘法器，因为咱自己实现乘法器的原理不一定比综合器依据的原理更好。
> 4. 后续工作是补充（1）中提到的内容以及（3）中提到的两个“要么”。
