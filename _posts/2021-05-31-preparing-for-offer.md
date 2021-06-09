---
layout: post
title: Preparing for Offer
author: Liang Chen
date: 2021-05-31 18:00:00 +0800
tags: [Notes, Offer]
catalog: true
---

## 1. CDC(clock domain crossing)

> [**Clock Domain Crossing (CDC) Design & Verification Techniques Using SystemVerilog**](http://www.sunburst-design.com/papers/CummingsSNUG2008Boston_CDC.pdf)
>
> [https://www.zhihu.com/people/li-hong-jiang-54/posts](https://www.zhihu.com/people/li-hong-jiang-54/posts)

### 1.0 HANDSHAKE SYNCHRONIZER

![handshake]({{site.url}}/img/in-post/notes/handshake.png)

![handshake-timing-graph]({{site.url}}/img/in-post/notes/handshake-timing-graph.png)

### 1.1 Single Bit

1. MTBF(mean time before failure)

2. Two flip-flop synchronizer

    - output by 1-level DFF in sending clock domain
    
    - 2-level DFF in receiving clock domain

    ![single-bit-cdc]({{site.url}}/img/in-post/notes/single-bit-cdc.png)

3. Problems with passing fast CDC pulse

    - When passing CDC signal between two clock domains through a 2-sync flop, CDC signal must be **1.5 times** the cycle width of the receiving clock domain. 
    
    - Make sure CDC signal pulse width is 1.5X the period of the receiving domain clock.
    
    - Send the syncchronization feedback signal to sending clock domain.

4. Passing multiple signals between clock domains

    - Consolidate multi-bit CDC signals into 1 CDC signal.

    - Encode multiple CDC bits using **Grey code** and then pass.

    - MCP(multi cycle path)

    - Async FIFO

### 1.2 Multi-Bit

#### 1.2.1 MCP(multi cycle path)

#### 1.2.2 Async FIFO(first in first out)

> [https://www.youtube.com/watch?v=0LVHPRmi88c](https://www.youtube.com/watch?v=0LVHPRmi88c)

![fifo]({{site.url}}/img/in-post/notes/fifo.png)

1. FIFO depth calculation

    ![fifo-depth]({{site.url}}/img/in-post/notes/fifo-depth.png)

2. Gray encoding & decoding

    - Encoding: Gray = Binary xor (Binary >> 1)

    - Example:

        ```verilog
        B101
        G = 101 ^ 010 = 111
        ```

    - Decoding: Binary[i] = xor(Gray >> i)

    - Example:

        ```verilog
        G111
        B[0] = ^(111 >> 0) = ^(111) = 1
        B[1] = ^(111 >> 1) = ^(011) = 0
        B[2] = ^(111 >> 2) = ^(001) = 1
        B = 101
        ```

3. Write_Full & Read_Empty Flag

    - Binary pointer

        ```verilog
        Empty: r_ptr = w_ptr
        Full: r_ptr = {~w_ptr[MSB], w_ptr[MSB-1:0]}
        ```

    - Gray pointer

        ```verilog
        Empty: r_ptr = w_ptr
        Full: r_ptr = {~w_ptr[MSB], ~w_ptr[MSB-1], w_ptr[MSB-2:0]}
        ```

        | Decimal | Binary  | Gray    |
        | :----:  | :----:  | :----:  |
        | **4'd0**    | **4'b0000** | **4'b0000** |
        | 4'b1    | 4'b0001 | 4'b0001 |
        | 4'b2    | 4'b0010 | 4'b0011 |
        | 4'b3    | 4'b0011 | 4'b0010 |
        | 4'b4    | 4'b0100 | 4'b0110 |
        | 4'b5    | 4'b0101 | 4'b0111 |
        | 4'b6    | 4'b0110 | 4'b0101 |
        | 4'b7    | 4'b0111 | 4'b0100 |
        | **4'b8**    | **4'b1000** | **4'b1100** |
        | 4'b9    | 4'b1001 | 4'b1101 |
        | 4'b10   | 4'b1010 | 4'b1111 |
        | 4'b11   | 4'b1011 | 4'b1110 |
        | 4'b12   | 4'b1100 | 4'b1010 |
        | 4'b13   | 4'b1101 | 4'b1011 |
        | 4'b14   | 4'b1110 | 4'b1001 |
        | 4'b15   | 4'b1111 | 4'b1000 |

4. FIFO DEPTH CONSIDERATION

    - sync fifo: 9, 10, ..., anything

    - async fifo: **only power of 2 ??**

    - Example:

        ```
        Depth: 2exp3 = 8
        Binary: 000  001 010 011 100 101 110  111
        Gray:  *000* 001 011 010 110 111 101 *100* ---> 000 (only one bit changed)
        /////////////////////////////////////////////////////////////////////////////// 
        Supposed FIFO depth is 6
        Binary: 000  001 010 011 100 101
        Gray:  *000* 001 011 010 110 111 ---> 000 (multi-bits changed) cause problems
        ///////////////////////////////////////////////////////////////////////////////
        Gray code addr start: 2exp(n-1) - FIFO_DEPTH/2
        Gray code addr end:   2exp(n-1) + FIFO_DEPTH/2 - 1
        ////////////////////////////////////////////////////////////////////////////// 
        Supposed FIFO depth is 6
        Gray code addr start: 2exp(n-1) - FIFO_DEPTH/2     = 2exp(3-1) - 6/2    = 1
        Gray code addr end:   2exp(n-1) + FIFO_DEPTH/2 - 1 = 2exp(3-1) + 6/2 -1 = 6
        ==>
        Binary: 001  010 011 100 101 110
        Gray:  *001* 011 010 110 111 101 ---> 001 (only one bit changed)
        ```

## 2. STA(static timing analysis)

1. Combinational circuits

    ![sta-combin-0]({{site.url}}/img/in-post/notes/sta-combin-0.png)
    
    ![sta-combin-1]({{site.url}}/img/in-post/notes/sta-combin-1.png)
    
    ![sta-combin-2]({{site.url}}/img/in-post/notes/sta-combin-2.png)

2. Sequential circuits

    ![sta-sequ-0]({{site.url}}/img/in-post/notes/sta-sequ-0.png)

    ![sta-sequ-1]({{site.url}}/img/in-post/notes/sta-sequ-1.png)

    ![sta-sequ-2]({{site.url}}/img/in-post/notes/sta-sequ-2.png)

3. Metastability

    ![sta-meta-0]({{site.url}}/img/in-post/notes/sta-meta-0.png)

    ![sta-meta-1]({{site.url}}/img/in-post/notes/sta-meta-1.png)

## 3. FSM(finite-state machine)

### 3.1 Moore fsm

output values are determined only by its **current state**.

### 3.2 Mealy fsm

output values are determined both by its **current state and the current inputs**.

### 3.3 2-level 

```verilog
module fsm(...);

always @(posedge clk or negedge rst_n)begin
  if (!rst_n)
    current_state <= IDLE;
  else
    current_state <= next_state;
end

always @(*)begin
  next_state = x;
  case(current_state)
    IDLE:begin next_state = S1;end
    S1:  begin next_state = S2;end
    S1:  begin next_state = S2;end
    S3:  begin next_state = IDLE;end
    default: ;
  endcase
end

always @(*)begin
  out_c = 0;
  out_d = 0;
  case(current_state)
    S2:begin out_c = 1;end
    S3:begin out_d = 1;end
    default: ;
  endcase
end

always @(posedge clk or negedge rst_n)begin
  if (!rst_n)begin
    REG_out_a <= 0;
    REG_out_b <= 0;
  end
  else begin
    case(current_state)
    IDLE:begin ... end
    S1:  begin REG_out_a <= 1;end
    S2:  begin REG_out_b <= 1;end
    default: ;
    endcase
  end
end

endmodule
```

### 3.4 3-level

```verilog
module fsm(...);

always @(posedge clk or negedge rst_n)begin
  if (!rst_n)
    current_state <= IDLE;
  else
    current_state <= next_state;
end

always @(*)begin
  next_state = x;
  case(current_state)
    IDLE:begin next_state = S1;end
    S1:  begin next_state = S2;end
    S1:  begin next_state = S2;end
    S3:  begin next_state = IDLE;end
    default: ;
  endcase
end

always @(posedge clk or negedge rst_n)begin
  if (!rst_n)begin
    REG_out_a <= 0;
    REG_out_b <= 0;
  end
  else begin
    case(next_state)
    IDLE:begin ... end
    S1:  begin REG_out_a <= 1;end
    S2:  begin REG_out_b <= 1;end
    default: ;
    endcase
  end
end

always @(*)begin
  out_c = 0;
  out_d = 0;
  case(next_state)
    S2:begin out_c = 1;end
    S3:begin out_d = 1;end
    default: ;
  endcase
end

endmodule
```

![fsm]({{site.url}}/img/in-post/notes/fsm.png)

## 4. Project Experience

### 4.1 vcs sim script

[https://liangchen01xz.github.io/2021/01/18/vcs-sim-scr/](https://liangchen01xz.github.io/2021/01/18/vcs-sim-scr/)

### 4.2 lstm

[https://liangchen01xz.github.io/2020/11/25/Paper-Notes/](https://liangchen01xz.github.io/2020/11/25/Paper-Notes/)

[https://liangchen01xz.github.io/2020/12/01/Fixed-Point-Floating-Point-Notes/](https://liangchen01xz.github.io/2020/12/01/Fixed-Point-Floating-Point-Notes/)

### 4.3 **spi2apb**

#### 4.3.1 Summary

1. 8bits paddr and pwdata & prdata in spi_slave module(download online);
   32bits paddr and pwdata & prdata in SoC module(self).

2. Three apb slaves included ddr3 controller, ddr3 phy and SoC modules;
   but only one prdata net in design top, causing net prdata has multi-drivers.
   Testbench using verilog(wire prdata) only reports Warning but no Error,
   causes simulation continue until `$finish()`.
   Testbench using systemverilog(logic prdata) reports Error, and simulation interrupts.
   Firstly, add Multiplexer depending dirrerent address used by three slaves for prdata: bad result;
   Secondly, for prdata and all other p_signals including psel, penable, pwrite, pwdata, paddr: good result.

#### 4.3.2 APB2P0

> [https://developer.arm.com/documentation/ihi0024/latest/](https://developer.arm.com/documentation/ihi0024/latest/)

![apb-about]({{site.url}}/img/in-post/notes/apb-about.png)

![apb-signals-table0]({{site.url}}/img/in-post/notes/apb-signals-table0.png)

![apb-signals-table1]({{site.url}}/img/in-post/notes/apb-signals-table1.png)

![apb-address-bus]({{site.url}}/img/in-post/notes/apb-address-bus.png)

![apb-data-buses]({{site.url}}/img/in-post/notes/apb-about.png)

![apb2p0-table]({{site.url}}/img/in-post/notes/apb2p0-table.png)

![apb-fsm]({{site.url}}/img/in-post/notes/apb-fsm.png)

![apb-write]({{site.url}}/img/in-post/notes/apb-write.png)

![apb-write-wait]({{site.url}}/img/in-post/notes/apb-write-wait.png)

![apb-read]({{site.url}}/img/in-post/notes/apb-read.png)

#### 4.3.3 SPI(Serial Peripheral Interface)

> [https://www.corelis.com/education/tutorials/spi-tutorial/](https://www.corelis.com/education/tutorials/spi-tutorial/)

![spi-4wire]({{site.url}}/img/in-post/notes/spi-4wire.png)

![spi-mode]({{site.url}}/img/in-post/notes/spi-mode.png)

![spi-write]({{site.url}}/img/in-post/notes/spi-write.png)

![spi-read]({{site.url}}/img/in-post/notes/spi-read.png)

### 4.4 DC(Design Compiler)

### 4.5 backend(Innovus)

## 5. Others

### 5.1 Sequence detector

### 5.2 Vending machine

### 5.3 Matrix keyboard & Key debounce

### 5.4 Asynchronous reset & Synchronous reset & Reset synchronizer

1. Recovery time

2. Removal time

3. Sync reset

    ![rst-sync]({{site.url}}/img/in-post/notes/rst-sync.png)

    ![rst-sync-wave]({{site.url}}/img/in-post/notes/rst-sync-wave.png)

    ```verilog
    module sync_rst(input clk, input rst_n, input data_in, output data_out);
    reg data_out;
    always @(posedge clk)
      if (!rst_n)
        data_out <= 0;
      else 
        data_out <= data_in;
    endmodule
    ```

4. Async reset

    ![rst-async]({{site.url}}/img/in-post/notes/rst-async.png)

    ![rst-async-wave]({{site.url}}/img/in-post/notes/rst-async-wave.png)

    ```verilog
    module async_rst(input clk, input rst_n, input data_in, output data_out);
    reg data_out;
    always @(posedge clk or negedge rst_n)
      if (!rst_n)
        data_out <= 0;
      else 
        data_out <= data_in;
    endmodule
    ```

5. Async reset sync release

    ![rst-syncer]({{site.url}}/img/in-post/notes/rst-syncer.png)

    ![rst-syncer-wave]({{site.url}}/img/in-post/notes/rst-syncer-wave.png)

    ```verilog
    module reset_syncer(input clk, input rst_n, input data_in, output data_out);
    reg  data_out;
    reg  sync_rst_n_1;
    reg  sync_rst_n_2;
    wire sync_rst_n;

    always @(posedge clk or negedge rst_n)
      if (!rst_n)begin
        sync_rst_n_1 <= 0;
        sync_rst_n_2 <= 0;
      end
      else begin
        sync_rst_n_1 <= 1;
        sync_rst_n_2 <= sync_rst_n_1;
      end
    assign sync_rst_n <= sync_rst_n_2;

    always @(posedge clk)
      if (!sync_rst_n)
        data_out <= 0;
      else
        data_out <= data_in;

    endmodule
    ```
