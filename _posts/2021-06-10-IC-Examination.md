---
layout: post
title: IC Examination
author: Liang Chen
date: 2021-06-01 18:00:00 +0800
tags: [IC, offer]
catalog: true
---

1. 乐鑫科技提前批

    - 上采样数据时，采用（高通/低通）滤波器，目的是（）

    - 芯片温度较（高/低），工作电压（高/低），速度越快

    - systemverilog中function语句能否可以被综合？

    - UVM的顶层文件是（uvm_root/uvm_top）？ 

    - A, B, C是复数：`|A * (B + C)|^2`运算用到了（）个乘法器，（）个加法器？

    - 4 bits and 8 bits signals inputs, 内部计算时最大位宽是（）？

    - `a=4'b10x1, b=4'b10x1`, `a == b`是（），`a === b`是（）？

    - Async BUS, Sync BUS (SPI UART USB IIC)

    - 设计电路，求当有效信号拉高时，输入序列的次小值是多少，以及次小值出现的次数？

        ```verilog
        module 2nd_smallest_value(
          input       clk,
          input       rst_n,
          input       d_vld,
          input  [9:0] d_in,
          output [9:0] d_out,
          output [8:0] cnt
        );

        // your answer


        endmodule
    ```
