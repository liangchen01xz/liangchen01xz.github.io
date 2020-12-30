---
layout: post
title: 芯动力听课笔记
subtitle:
author: Liang Chen
lang: en
date: 2020-10-23 18:00:00 +0800
tags: [Notes, IC]
catalog: true
mathjax: true
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

> 课程地址：[芯动力-硬件加速设计方法](https://www.icourse163.org/course/SWJTU-1207492806)

1. 可综合四大法宝：`always`, `if-else`, `case`, `assign`

2. `if-else` 映射多路选择器（MUX），“先加后选，先选后加“

3. **单 if 语句没有优先级**，`if-else-if-else...`

4. **多 if 语句最后一级优先级最高**，`if-if-if...`，消耗组合逻辑，**最高优先级给最迟到达的信号**

5. **case 语句没有优先级**，条件互斥，指令译码，`// synopsys full_case`, `// synopsys parallel_case`

6. 慎用 latch，电平触发，非同步控制，不能过滤毛刺（glich），静态时序分析复杂，能用 DFF，就不用 latch，**latch会以 Warning 的形式报告**

7. 逻辑复制，降低关键信号的扇出

8. 资源共享 ，减小面积

9. 顺序重排，降低传播延时

10. **`:?` 用于连线，`always` 用于逻辑运算，`assign` 用于信号连接**

11. always 块敏感信号列表一定要完备

12. **？？？在综合过程中，每个 always 块敏感信号列表对应一个时钟，原因：这是将每一个过程限制在单一寄存器类型的要求**

13. 分开异步逻辑和同步逻辑，分开控制逻辑和存储器

14. 在RTL编码中考虑时延

15. 在RTL编码中考虑面积，触发器，加法器，乘法器，触发器的数量由功能决定，很难减少，组合逻辑描述时的各种操作符，条件语句中的比较运算，资源共享，考虑多比特信号的所有比特是不是都需要进行操作，能不能只针对部分比特进行操作，因为多比特意味着成倍的使用资源，逻辑表达式的简化

16. 在RTL编码中考虑功耗，各点功耗的总和`Pd`，该点电路的翻转次数`a`，**在RTL级就是降低电路信号的翻转频率**

    $$
    P_d = \sum {afCV^2}
    $$

    - **？？？门控时钟**
    - 使能信号
    - 对芯片的各个模块进行控制
    - 有用信号和时钟的翻转会提供功耗，组合逻辑产生的毛刺会提供大量功耗，尽量把毛刺电路放在传播路径的最后，采取降低毛刺的技术
    - 有限FSM，低功耗编码，状态机，译码器，多路选择器

17. 在RTL中考虑布线，**热点：设计的功能需要在一个面积内占用大量的布线资源**，RTL采用了特定的结构，比如很大的MUX，可以拆分成小MUX

18. RTL设计原则：面积速度互换，乒乓操作，流水线设计

19. FPGA 面积指标可以用所消耗的触发器和查找表数量，ASIC 面积指标可以用设计的系统门衡量，速度就是最高频率

20. 减小面积 ：**功能模块复用**；提高频率：**串并转换**，**乒乓操作**，复制多个操作模块，在输出模块并串转换

21. 乒乓操作常用存储单元：双口RAM（DPRAM），单口RAM（SPRAM），FIFO

22. 乒乓操作对数据进行流水线式处理

23. 乒乓操作：输入数据选择模块 数据缓存模块 输出数据选择模块 数据处理模块 

24. 乒乓操作节约缓冲区空间，用低速模块处理高速数据流

25. 系统时钟频率取决于最长的组合逻辑

26. **流水线**使得大部分电路都在工作，提高数据吞吐量，系统时钟取决于最慢的流水线级的延时，过多的级数不一定能产生最快的结果，太多寄存器的插入会导致芯片面积增加，布线困难， 时钟偏差增加

--------------------------------------------------------------------------------------

## 低功耗设计

[概述](https://zhuanlan.zhihu.com/p/137937714)

[门控技术](https://zhuanlan.zhihu.com/p/139363948)

$$
E(能量) = W(功耗) \ast t(时间)
$$

**低功耗设计不是低能量设计**

功耗越大，使用时间越短；产生热量越多，需要及时散热，散热系统要求高；封装要求高

功耗：动态功耗；静态功耗

动态功耗：信号改变的功耗；静态功耗：设备上电的功耗

动态功耗：翻转功耗（Switching Power）；短路功耗（Internal Power）

翻转功耗：门电路对输出电容进行充放电的功耗，是动态功耗的主要组成部分

$$
P_{dyn} = \sum {afCV^2} + [(t_{sc} \ast V_{dd} \ast I_{peak} \ast f)](静态功耗忽略不计)
$$

静态功耗：由漏电流引起，CMOS门中，亚阈值漏电流；栅极漏电流；【栅极感应漏电流；反向偏置结泄漏】（忽略不计）

$$
亚阈值漏电流：I_{sub} = \mu \ast C_{ox} \ast V_{th}^2 \ast (W/L) \ast e^{(\frac{V_{GS}-V_T}{nV_{th}})}
$$

RTL级设计改变不了静态功耗

栅极漏电流：使用高 K 介质

总的静态功耗表达式：

$$
P_{peak} = V_{DD} \ast I_{peak}
$$

SoC 中的功耗大户：时钟树（40%）；CPU；GPU / AI ；存储器（DDR）

**门控时钟（Clock Gating）**

结构：EN 信号通过 Clk 钟控的锁存器/寄存器，再与 Clk 信号相与，得到 GCLK 信号 

锁存门控；寄存门控；

综合器自动综合成锁存门控

DC 默认输入信号数据位宽超过3时，自动插入门控时钟

关键DC命令，调整最小位宽：`set_clock_gating_style -minimum_bitwidth 4`

RTL级门控时钟代码编写风格：

```verilog
always @(posedge clk or negedge rstn)begin
    if(rstn)
        dout <= 0;
    else if(valid)
        dout <= din;
//    else
//        dout <= 0;
end
```

-------------------------------------------------------------------

> [乒乓操作，串并转换，并串转换，流水线](https://zhuanlan.zhihu.com/p/63279853)
>
> [乒乓操作](https://www.cnblogs.com/lai-jian-tao/p/10796248.html)

--------------------------------------------------------------

> [参考1](https://halftop.github.io/post/verilog-day5/#toc-heading-5)
>
> [参考2](https://blog.csdn.net/phenixyf/article/details/8727775)
> 
> [参考3](https://zhuanlan.zhihu.com/p/34974464)

1. 锁存器：非同步控制，电平触发，容易产生毛刺，ASIC中使用，RS锁存器（2个NAND门）~> 钟控RS锁存器 ~> 钟控D锁存器 （4个NAND门）

2. 触发器：同步控制，边沿触发，FPGA中有DFF单元，没有标准的LATCH单元，DFF（8个NAND门）由2个D锁存器构成

3. LATCH 和 DFF 都是时序逻辑，输出与当前输入和上一状态的输出有关

4. NAND门的晶体管结构组成

![DFF]({{site.url}}/img/in-post/notes/DFF.jpg)

![CMOS]({{site.url}}/img/in-post/notes/CMOS.jpg)

![wave]({{site.url}}/img/in-post/notes/wave.png)

