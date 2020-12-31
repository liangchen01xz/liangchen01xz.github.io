---
layout: post
title: Paper笔记 
subtitle:
author: Liang Chen
lang: en
date: 2020-11-25 18:00:00 +0800
tags: [Notes, Paper]
catalog: true
mathjax: true
---


## Convolutional LSTM Network: A Machine Learning Approach for Precipitation Nowcasting

**Keyword:** spatiotemporal sequence 

1. The key equations of LSTM are:

    $$
    where\ \circ\ denotes\ the\ Hadamard\ product,\ also\ known\ as\ element-wise\ multiplication.
    $$
    
    $$
    \begin{align*}
    & i_t = \sigma (W_{xi}x_t + W_{hi}h_{t-1} + W_{ci}c_{t-1} +b_i) \\
    &                                                               \\
    & f_t = \sigma (W_{xf}x_t + W_{hf}h_{t-1} + W_{cf}c_{t-1} +b_f) \\
    &                                                               \\
    & \bar{c_t} = tanh(W_{xc}x_t + W_{hc}h_{t-1} + b_c)             \\
    &                                                               \\
    & c_t = f_t \circ c_{t-1} + i_t \circ \bar{c_t}                 \\
    &                                                               \\
    & o_t = \sigma ( W_{xo}x_t + W_{ho}h_{t-1} + W_{co}c_{t} +b_o ) \\
    &                                                               \\
    & h_t = o_t \circ tanh(c_t)
    \end{align*}
    $$

2. The key equations of ConvLSTM are:

    $$
    where\ \ast\ denotes\ the\ convolution\ operator\ and\ \circ\ denotes\ the\ Hadamard\ product,\ also\ known\ as\ element-wise\ multiplication.
    $$
    
    $$
    \begin{align*}
    & i_t = \sigma (W_{xi} \ast X_t + W_{hi} \ast H_{t-1} + W_{ci} \circ C_{t-1} +b_i) \\
    &                                                                                  \\
    & f_t = \sigma (W_{xf} \ast X_t + W_{hf} \ast H_{t-1} + W_{cf} \circ C_{t-1} +b_f) \\
    &                                                                                  \\
    & \bar{C_t} = tanh(W_{xc} \ast X_t + W_{hc} \ast H_{t-1} + b_c)                    \\
    &                                                                                  \\
    & C_t = f_t \circ C_{t-1} + i_t \circ \bar{C_t}                                    \\
    &                                                                                  \\
    & o_t = \sigma ( W_{xo} \ast X_t + W_{ho} \ast H_{t-1} + W_{co} \circ C_{t} +b_o ) \\
    &                                                                                  \\
    & H_t = o_t \circ tanh(C_t)
    \end{align*}
    $$

## Long Exposure Convolutional Memory Network for accurate estimation of finger kinematics from surface electromyographic signals (Publishing)

**Keyword:** LE-ConvMN

1. Retains the spatial and temporal information

2. Estimate the continuous movement of fingers with sEMG

3. More recent literature began to shift research focus from discrete movement classification to continuous movement regression

4. EMG-based continuous motion estimation approaches can be categorized as model-based and model-free approaches

5. This approach builds the connection between sEMG recordings and hand kinematics

6. LSTM has been proved more capable on the regression problem or continuous output fitting problem

7. The key equations of LSTM are: ...

8. The key equations of ConvLSTM are: ...

9. The structure of LE-ConvMN:

    ![]({{site.url}}/img/in-post/notes/LEConvMN.png)

## ESE: Efficient Speech Recognition Engine with Sparse LSTM on FPGA

***Chapter5(HARDWARE IMPLEMENTATION) is very important!!!***

**Keyword:** ESE

1. ***memory footprint***

    - Memory footprint refers to the amount of main memory that a program uses or references while running.
    - The word footprint generally refers to the extent of physical dimensions that an object occupies, giving a sense of its size.
    - In computing, the memory footprint of a software application indicates its runtime memory requirements, while the program executes.

2. ***memory reference***

    - These instructions refer to memory address as an operand. The other operand is always accumulator.
    - Specifies 12-bit address, 3-bit opcode (other than 111) and 1-bit addressing mode for direct and indirect addressing.

3. ESE optimizes LSTM computation across algorithm, software and hardware stack:

    ![]({{site.url}}/img/in-post/notes/design_flow.png)

4. The speech recognition pipeline. LSTM takes more than 90% of the total execution time in the whole computation pipeline:

    ![]({{site.url}}/img/in-post/notes/speech_recognition_pipeline.png)

5. Load Balance-Aware Pruning

6. Weight and Activation Quantization: [神经网络模型量化方法简介](https://chenrudan.github.io/blog/2018/10/02/networkquantization.html)

7. Encoding in CSC format and data align using zero-padding

    ![]({{site.url}}/img/in-post/notes/data_align.png)

8. CSC: compressed sparse column

9. SpMV: sparse matrix vector multiplication

10. El-emMul: element-wise multiplication

11. diagonal matrix: 对角矩阵

12. **the overall ESE system**

    ![]({{site.url}}/img/in-post/notes/overall_ESE.png)

13. **one channel with multiple PEs**

    ![]({{site.url}}/img/in-post/notes/one_channel.png)

14. **the state flow of ESE**

    ![]({{site.url}}/img/in-post/notes/state_flow.png)

15. **Memory management unit**

    ![]({{site.url}}/img/in-post/notes/memory_management.png)

## 嵌入式人工智能处理器循环神经网络模块设计

1. Activation Function: [https://towardsdatascience.com/activation-functions-neural-networks-1cbd9f8d91d6](https://towardsdatascience.com/activation-functions-neural-networks-1cbd9f8d91d6)

    - [Sigmoid](https://en.wikipedia.org/wiki/Sigmoid_function)
    - [Tanh](https://sefiks.com/2017/01/29/hyperbolic-tangent-as-neural-network-activation-function/)
    - [Relu](https://towardsdatascience.com/activation-functions-neural-networks-1cbd9f8d91d6)

2. 可重构计算: [Reconfigurable computing](https://en.wikipedia.org/wiki/Reconfigurable_computing)

3. 边缘计算: [知乎链接](https://www.zhihu.com/question/62869157)

4. 可重构芯片: [知乎文章](https://zhuanlan.zhihu.com/p/76386579)

5. RNN

    - LSTM: [知乎文章](https://zhuanlan.zhihu.com/p/32085405)
    - GRU: [知乎文章](https://zhuanlan.zhihu.com/p/32481747)

6. ALU: [Arithmetic Logic Unit](https://study.com/academy/lesson/arithmetic-logic-unit-alu-definition-design-function.html)

