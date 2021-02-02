---
layout: post
title: Verilog课程设计
subtitle: 64x64bits unsigned integer multipiler
author: Liang Chen
date: 2021-02-02 18:00:00 +0800
tags: [Notes, verilog, mult]
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

Project Address: [https://github.com/cliang935/verilog_course_design](https://github.com/cliang935/verilog_course_design)

> 作者按：
> 1. 这是研一时一门课的课程设计，实现的功能是64x64位无符号整数乘法器，并且对其进行了前仿真、FPGA综合后仿真等操作。方案1和方案2是利用了16x16乘法器的小IP，时分复用，方案3同上一篇博客的设计。**这个只是我的一个课设，写在这里做个备份，博君一笑。**
> 2. 局限性：
      - 移位相加这样写：
      ```verilog
      row1 = p11 + (p12 << 16) + (p13 << 32) + (p14 << 48);
      ......
      ```
      - 门级仿真的输入输出延迟不确定是单cycle的整数倍周期，还是无所谓多长的时间吗？
> 3. PPA原则：从这个简单的设计仿真结果来看，果然是频率高了，面积就大了；面积小了，频率就低了。
