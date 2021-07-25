---
layout: post
title: Asynchronous FIFO
author: Liang Chen
date: 2021-07-25 18:00:00 +0800
tags: [IC, FIFO]
catalog: true
---

> Reference: [https://www.cnblogs.com/mikewolf2002/p/10945488.html](https://www.cnblogs.com/mikewolf2002/p/10945488.html)

```verilog
// HAVE BUG, WAITING FOR DEBUG !!!
module async # (
  parameter DATA_SIZE    = 8,
            ADDRESS_SIZE = 4 
)
(
  input                  wclk, wrst_n, wq,
  input                  rclk, rrst_n, rq,
  input  [DATA_SIZE-1:0] wdata,
  output [DATA_SIZE-1:0] rdata,
  output reg             wfull, rempty

);

reg [DATA_SIZE-1:0]    mem [0:(1<<ADDRESS_SIZE)-1];
reg [ADDRESS_SIZE-1:0] waddr_bin, raddr_bin;
wire[ADDRESS_SIZE-1:0] waddr_gray, raddr_gray;
reg [ADDRESS_SIZE-1:0] wptr, rptr;
reg [ADDRESS_SIZE-1:0] w1_rptr, w2_rptr;
reg [ADDRESS_SIZE-1:0] r1_wptr, r2_wptr;

assign rdata = mem[raddr_bin];

always @ (posedge wclk)
begin
  if (wq && !wfull)
    mem[waddr_bin] <= wdata;
end

always @ (posedge wclk or negedge wrst_n)
begin
  if (!wrst_n)
  begin
    w1_rptr <= 0;
    w2_rptr <= 0;
  end
  else
  begin
    w1_rptr <= rptr;
    w2_rptr <= w1_rptr;
  end
end

always @ (posedge rclk or negedge rrst_n)
begin
  if (!rrst_n)
  begin
    r1_wptr <= 0;
    r2_wptr <= 0;
  end
  else
  begin
    r1_wptr <= wptr;
    r2_wptr <= r1_wptr;
  end
end

always @ (posedge rclk or negedge rrst_n)
begin
  if (!rrst_n)
  begin
    raddr_bin <= 0;
    rptr      <= 0;
  end
  else
  begin
    if (rq & !rempty)
    begin
      raddr_bin  <= raddr_bin + 1'b1;
      rptr       <= raddr_gray;
    end
  end
end

assign raddr_gray = raddr_bin ^ (raddr_bin >> 1);

always @ (posedge rclk or negedge rrst_n)
begin
  if (!rrst_n)
    rempty <= 1'b1;
  else
    rempty <= (rptr==r2_wptr);
end

always @ (posedge wclk or negedge wrst_n)
begin
  if (!wrst_n)
  begin
    waddr_bin <= 0;
    wptr      <= 0;
  end
  else
  begin
    if (wq & !wfull)
    begin
      waddr_bin <= waddr_bin + 1'b1;
      wptr      <= waddr_gray;
    end
  end
end

assign waddr_gray = waddr_bin ^ (waddr_bin>>1);

always @ (posedge wclk or negedge wrst_n)
begin
  if (!wrst_n)
    wfull <= 1'b0;
  else
    wfull <= (w2_rptr == {~wptr[ADDRESS_SIZE-1], ~wptr[ADDRESS_SIZE-2], wptr[ADDRESS_SIZE-3:0]});
end

endmodule
```
