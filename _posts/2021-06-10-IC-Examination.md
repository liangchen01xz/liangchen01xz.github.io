---
layout: post
title: IC 笔试复盘
author: Liang Chen
date: 2021-06-10 18:00:00 +0800
tags: [IC, offer]
catalog: true
---

<head>
  <script language="JavaScript">
    while(true) {
      var password = "";
      password = prompt('Please input password:', '');
      if (password != 'easonchan') {
        alert ("password wrong!!");
      } else {
        break;
      }
    }
  </script>
</head>

1. 某鑫提前批

    - 上采样数据时，采用（高通/低通）滤波器，目的是（）

        低通滤波器；抗混叠

    - 芯片温度较（高/低），工作电压（高/低），速度越快

        温度越高，工作电压越高，速度越快  
        温度升高，阈值电压降低，工作电压越高，越容易超过阈值电压

    - systemverilog中function语句能否可以被综合

        Verilog and SystemVerilog tasks and functions are synthesizable.

    - UVM的顶层文件是（uvm_root/uvm_top）

        `uvm_root`  
        `uvm_root` is class, `uvm_top` is instance or global variable?  
        [https://verificationacademy.com/verification-methodology-reference/uvm/docs_1.2/html/files/base/uvm_root-svh.html](https://verificationacademy.com/verification-methodology-reference/uvm/docs_1.2/html/files/base/uvm_root-svh.html)

    - A, B, C是复数：`|A * (B + C)|^2`运算用到了（）个乘法器，（）个加法器
      
        10 MULs, 5 Adds

    - 4 bits and 8 bits signals inputs, 内部计算时最大位宽是（）

        12 ??? 根据计算方法的不同，内部寄存器的位宽不同，最大位宽那就是 8bits = 4 + 4 ??

    - `a=4'b10x1, b=4'b10x1`, `a == b`是（），`a === b`是（）

        ```
        (a==b)  = x
        (a===b) = 1 
        ```

        For the logical equality and logical inequality operators (== and !=), if, due to unknown or high-impedance bits in the operands, the relation is ambiguous, then the result shall be a 1-bit unknown value (x).  
        For the case equality and case inequality operators (=== and !==), the comparison shall be done just as it is in the procedural case statement. Bits that are x or z shall be included in the comparison and shall match for the result to be considered equal. The result of these operators shall always be a known value, either 1 or 0.

    - Async BUS, Sync BUS (SPI UART USB IIC)

        Synchonization: SPI IIC (USB???)  
        Asynchonization: UART

    - verilog单导线仿真模型如何表示惯性延迟和传输延迟5ns

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

    - 设计电路，求当有效信号拉高时，输入序列的次小值是多少，以及次小值出现的次数

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

---------------------------------------------------------------------------------------

2. 某为提前批

    - SV中，哪种数组使用前需要new？【D】  
        A. packed array  
        B. associative arrays  
        C. 多维数组  
        D. dynamic arrays  

    - src为0的概率是多少？【B】  
        ```
         constraint c_0 {
           src dist {0:=30, [1:3]:=90};
         }
        ```
        A. 0.6  
        B. 0.1  
        C. 0.2  
        D. 0.25  

    - 哪条语句是对的？【A】  
        A. `parameter BUS_WID=12;`  
        B. `wire buf;`  
        C. ``define AHB_TRANS_SEQ = 2'b11`  
        D. `reg [7:0] reg;`  

    - 有符号数右移操作符？【B】  
        A. `>>`  
        B. `>>>`  

    - Perl脚本中，退出当前循环使用？【B】  
        A. `exit`  
        B. `last`  
        C. `break`  
        D. `next`  

    - Moore和Mealy状态机的差异在？【D】  
        A. 状态和输入信号是否相关  
        B. 输出信号和状态是否相关  
        C. 状态和输出信号是否相关  
        D. 输出信号和输入信号是否相关  

    - 优先级最高的是？【】  
        A. !  
        B. ?:  
        C. &&  
        D. ==  

    - 4路数据选择器的信号输入端至少需要几根线？【】  
        A. 2  
        B. 8  
        C. 16  
        D. 32  

    - 优先级最高的是？【】  
        A. +  
        B. ||  
        C. >>  
        D. %  

    - 时钟的占空比是？【】  
        A. 高脉冲的持续时间与脉冲总周期的比值  
        B. 低脉冲的持续时间与脉冲总周期的比值  
        C. 时钟的变化范围  
        D. 时钟的变化速度  

    - 关于冯.诺伊曼结构和哈佛结构的描述错误的是？【】  
        A. 冯诺依曼接口中程序计数器负责提供程序执行所需要的地址  
        B. 哈佛结构的计算机在一个机器周期内可同时获得指令和操作数  
        C. 冯诺依曼结构的计算机中数据和程序共用一个存储空间  
        D. 哈佛结构中取指令和执行不能完全重叠  

    - 关于亚稳态的说法正确的是？【】  
        A. 信号无法满足setup要求是很可能出现亚稳态  
        B. 亚稳态打三拍才可以消除  
        C. 亚稳态打两拍才可以消除  
        D. 亚稳态打一拍就可以消除    

    - 关于低功耗的说法错误的是？【】  
        A. power gating降低功耗  
        B. 降低数据的翻转率降低功耗  
        C. clock gating降低功耗  
        D. 无论设计大小，一律采用先进工艺  

    - 关于Verilog过程赋值的说法错误的是？【B】  
        A. 在串行语句块中，下一语句的执行不会被本条阻塞型过程赋值语句所阻塞  
        B. 在begin-end串行语句块中，下一语句的执行会被本条非阻塞型过程赋值语句所阻塞    
        C. 赋值操作符`=`的过程赋值是阻塞型  
        D. 在非阻塞型过程赋值中，使用`<=`  

    - 不是组合逻辑的是？【C】  
        ```verilog
        // A:
        always @(*)
        begin
          if (cond_a==1'b1)
            data <= din1;
          else if (cond_b==1'b1)
            data <= din2;
          else
            data <= din3;
        end

        // B:
        assign data = (cond_a==1'b1) ? din1 : ((cond_b==1'b1) ? din2 : din3);

        // C:
        always @(posedge clk)
        begin
          if (cond_a==1'b1)
            data <= din1;
          else if (cond_b==1'b1)
            data <= data;
          else
            data <= din3;
        end

        // D:
        always @(din1 or din2 or din3 or cond_a or cond_b)
        begin
          if (cond_a==1'b1)
            data = din1;
          else if (cond_b==1'b1)
            data = din2;
          else
            data = din3;
        end
        ```

    - 优先级最低的是？【】  
        A. `>=`  
        B. `<<`  
        C. `~`  
        D. `*`  

    - STA中，每条Timing path都有一个起点和终点，它的终点可以是？【B】  
        A. 寄存器的Q端和CLK端  
        B. 芯片输出管脚和寄存器的D端  
        C. 芯片输出管脚和寄存器的CLK端  
        D. 芯片输出管脚和寄存器的Q端  
        [http://thuime.cn/wiki/images/0/03/STA_basic.pdf](http://thuime.cn/wiki/images/0/03/STA_basic.pdf)

    - 代码覆盖率达到100%，可以认为设计没有问题 【】  
        A. 正确  
        B. 错误  

    - 关于奇偶校验的说法，错误的是？【】  
        A. 采用偶校验可以检测偶数个bit错误  
        B. 如果数据包含有奇数个1，采用偶校验方式，则校验位是1  
        C. 如果数据包含有奇数个1，采用奇校验方式，则校验位是0  
        D. 采用奇校验可以检测奇数个bit错误  

    - `reg signed [0:4] c; c = 8'sh8f;`，c是多少？【】  
        A. 15  
        B. 17  
        C. -15  

    - PVT中的P是Process 【】  
        A. 错误  
        B. 正确  

    - A、B两个模块属于同一时钟域，分别可以访问一片two-port SRAM的两个端口，当A、B通过两个端口同时访问一个地址单元，则 【】  
        A. 可以正常写入，但读出数据可能写入前该单元的数据，也可能是最新的数据  
        B. 读写都可能失败  
        C. 可以正常读出，但不能写入
        D. 采用奇校验可以检测奇数个bit错误  

    - 有关复位的说法，正确的是？【D】  
        ```verilog
        always @(posedge clk or negedge rst_n)
        beign
          if (!rst_n)
            data <= 8'd0;
          else if (soft_rst==1'b0)
            data <= 8'd0;
          else
            data <= din;
        end
        ```

        A. rst_n是同步复位，soft_rst是同步复位  
        B. rst_n是同步复位，soft_rst是异步复位    
        C. rst_n是异步复位，soft_rst是异步复位  
        D. rst_n是异步复位，soft_rst是同步复位  

    - 寄存器如果出现亚稳态，其持续时间是？【】  
        A. 1个cycle  
        B. >1个cycle  
        C. <1个cycle    
        D. uncertain  

    - 32bit数0x01234567，类型为int，在32位系统中存储在地址0x100处，如果按Big-endian方式，地址0x100-0x103位置上存储的数值按16进制表示是？【D】  
        A. 0x76, 0x54, 0x32, 0x10  
        B. 0x67, 0x45, 0x23, 0x01  
        C. 0x10, 0x32, 0x54, 0x76  
        D. 0x01, 0x23, 0x45, 0x67  

    - 关于fork-join，错误的是？ 【C】  
        A. 有三个选项，all、any、none  
        B. fork-join any表示父进程至少等一个子进程执行完才能往下执行  
        C. fork-join none表示父进程需要等所有的子进程执行完才能往下执行  
        D. 默认是fork-join all  

    - systemverilog中类默认的属性是？【D】  
        A. private  
        B. automatic  
        C. local  
        D. public  

    - 占空比50%三分频时钟的电路，正确的是？【】  
        A. 待分频时钟上升沿采样计数，产生占空比1/3的分频时钟A；待分频时钟下降沿采样计数，产生占空比1/3的分频时钟B；A || B  
        B. 待分频时钟上升沿采样计数，产生占空比1/3的分频时钟A；待分频时钟上升沿采样计数，产生占空比2/3的分频时钟B；A && B  
        C. 待分频时钟上升沿采样计数，产生占空比1/3的分频时钟A；待分频时钟下降沿采样计数，产生占空比1/3的分频时钟B；A && B  
        D. 待分频时钟上升沿采样计数，产生占空比1/3的分频时钟A；待分频时钟上升沿采样计数，产生占空比2/3的分频时钟B；A || B  

    - 不属于动态功耗？【B】  
        A. 电路翻转功耗  
        B. 二极管反向电流引起的功耗  
        C. 电路短路功耗  

    - DFF，Tsetup = 2ns， Tc2q = 3ns，Thold = 1ns，则Fmax？【A】  
        A. 200MHz  
        B. 500MHz  
        C. 333MHz  
        D. 100MHz  

    - 【多选】Right choices? 【CD】  
        A. setup time and hold time violation must be considered in synthesis  
        B. hold time violation can be solved by decrease frequency of clock  
        C. setup time violation can be solved by decrease frequency of clock  
        D. hold time violation can be solved by increase frequency of clock  

    - 【多选】Which use one-hot fsm encode? 【AB】  
        A. 8'd16  
        B. 8'h20  
        C. 8'h16  
        D. 8'd20  

    - 【多选】Right choices about pipeline? 【AD】  
        A. pipeline increases area  
        B. pipeline increases delay of data, but outputthrough of system won't change if frequency of work does'nt change  
        C. pipeline certainly decreases area  
        D. pipeline is good to timing and STA  

    - 【多选】Which set test points with high priority? 【】  
        A. 验证工作量最大的测试点  
        B. 设计人员认为质量风险最大的测试点  
        C. 最常用功能相关的测试点  
        D. 用于内部调试的测试计数器等相关测试点  

    - 【多选】Which cause difficult analysis of STA? 【ABCD】  
        A. 组合逻辑环  
        B. 同一模块中存在大量异步逻辑  
        C. latch锁存器  
        D. 将时钟直接作为数据使用  

    - 【多选】Ways of instantiating module? 【BC】  
        A. 属性关联  
        B. 位置连接  
        C. 端口名称连接  

    - 【多选】Right choices about coverage rate? 【AC】  
        A. 代码覆盖率高，功能覆盖率低，需要加强功能点的覆盖  
        B. 功能覆盖率100%，代码覆盖率一定是100%  
        C. 代码覆盖率高，功能覆盖率高，往往标志验证正处于收敛状态，需要加强各边界点和异常点的测试  
        D. 代码覆盖率低，功能覆盖率高，往往是一个危险信号，说明功能覆盖率建模还不完善  

    - 【多选】Which behavior description statement can be synthesized? 【ABCD】  
        A. for loop  
        B. if-else  
        C. assign  
        D. always  

    - 【多选】Which influence static power consumption? 【ABCD】  
        A. process  
        B. operating temperature  
        C. operating voltage  
        D. load capacitance  
        E. 翻转活动因子  

    - 【多选】Difference of initial and always? 【AB】  
        A. initial执行一次，always执行多次  
        B. initial不能被综合，always可以被综合  
        C. always中时序和过程语句描述与initial相同  
