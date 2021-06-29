---
layout: post
title: IC Examination
author: Liang Chen
date: 2021-06-10 18:00:00 +0800
tags: [IC, offer]
catalog: true
---

1. 乐鑫科技提前批

    - *上采样数据时，采用（高通/低通）滤波器，目的是（）*

        低通滤波器；抗混叠

    - *芯片温度较（高/低），工作电压（高/低），速度越快*

        温度越高，工作电压越高，速度越快  
        温度升高，阈值电压降低，工作电压越高，越容易超过阈值电压

    - *systemverilog中function语句能否可以被综合*

        Verilog and SystemVerilog tasks and functions are synthesizable.

    - *UVM的顶层文件是（uvm_root/uvm_top）*

        `uvm_root`  
        `uvm_root` is class, `uvm_top` is instance or global variable?  
        [https://verificationacademy.com/verification-methodology-reference/uvm/docs_1.2/html/files/base/uvm_root-svh.html](https://verificationacademy.com/verification-methodology-reference/uvm/docs_1.2/html/files/base/uvm_root-svh.html)

    - *A, B, C是复数：`|A * (B + C)|^2`运算用到了（）个乘法器，（）个加法器*
        
        10 MULs, 5 Adds

    - *4 bits and 8 bits signals inputs, 内部计算时最大位宽是（）*

        12 ??? 根据计算方法的不同，内部寄存器的位宽不同，最大位宽那就是 8bits = 4 + 4 ??

    - *`a=4'b10x1, b=4'b10x1`, `a == b`是（），`a === b`是（）*

        ```
        (a==b)  = x
        (a===b) = 1 
        ```

        For the logical equality and logical inequality operators (== and !=), if, due to unknown or high-impedance bits in the operands, the relation is ambiguous, then the result shall be a 1-bit unknown value (x).  
        For the case equality and case inequality operators (=== and !==), the comparison shall be done just as it is in the procedural case statement. Bits that are x or z shall be included in the comparison and shall match for the result to be considered equal. The result of these operators shall always be a known value, either 1 or 0.

    - *Async BUS, Sync BUS (SPI UART USB IIC)*

        Synchonization: SPI IIC (USB???)  
        Asynchonization: UART

    - *verilog单导线仿真模型如何表示惯性延迟和传输延迟5ns*

        > [https://blog.csdn.net/kevindas/article/details/86777477](https://blog.csdn.net/kevindas/article/details/86777477)

        内定延迟：`out = #5 in;`  
        正规延迟：`#5 out = in;`  

        连续赋值  
            `assign out = #5 in;`:非法延迟，组合逻辑具有记忆特性    
            `assign #5 out = in;`:**惯性延迟**  
        阻塞赋值  
            `always @(*)begin out = #5 in; end`:二者既不能模拟惯性，也不能模拟传输延迟  
            `always @(*)begin #5 out = in; end`:不建议在阻塞赋值中插入延迟  

        非阻塞赋值  
            `always @(posedge clk or negedge rst_n)begin out <= #5 in; end`:**传输延迟**  
            `always @(posedge clk or negedge rst_n)begin #5 out <= in; end`:不建议  

        ```verilog
        // 惯性延迟
        `timescale 1ns/1ps
        module test_delay();
          wire out, in;
          assign #5 out = in;
        endmoule

        // 传输延迟
        `timescale 1ns/1ps
        module test_delay();
          wire in;
          reg  out;
          always @(posedge clk or negedge rst_n)
          begin
            out <= #5 in;
          end
        endmoule
        ```

    - *设计电路，求当有效信号拉高时，输入序列的次小值是多少，以及次小值出现的次数*

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
