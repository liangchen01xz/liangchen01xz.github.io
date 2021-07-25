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
  parameter FIFO_WIDTH  = 8,  
            FIFO_DEPTH  = 16,
            ADDR_WIDTH  = 4 
)
(
  input                   wclk, wrst_n, wq,
  input                   rclk, rrst_n, rq,
  input  [FIFO_WIDTH-1:0] wdata,
  output [FIFO_WIDTH-1:0] rdata,
  output                  wfull, rempty
);

reg  [FIFO_WIDTH-1:0] mem [FIFO_DEPTH-1:0];
wire [ADDR_WIDTH-1:0] waddr, raddr;
reg  [ADDR_WIDTH:0] waddr_bin, raddr_bin;
wire [ADDR_WIDTH:0] waddr_gray, raddr_gray;
reg  [ADDR_WIDTH:0] wptr, rptr;
reg  [ADDR_WIDTH:0] w1_rptr, w2_rptr;
reg  [ADDR_WIDTH:0] r1_wptr, r2_wptr;

assign waddr = waddr_bin[ADDR_WIDTH-1:0];
assign raddr = raddr_bin[ADDR_WIDTH-1:0];

always @ (posedge rclk or negedge rrst_n)
begin
  if (!rrst_n)
    rdata <= 0;
  else
  begin
    if (rq && !rempty)
      rdata <= mem[raddr];
    else
      rdata = rdata;
  end
end

always @ (posedge wclk)
begin
  if (wq && !wfull)
    mem[waddr] <= wdata;
  else
    mem[waddr] <= mem[waddr];
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

assign raddr_gray = raddr_bin ^ (raddr_bin >> 1);
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


assign waddr_gray = waddr_bin ^ (waddr_bin>>1);
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

assign rempty = (rptr==r2_wptr);
assign wfull = (w2_rptr == {~wptr[ADDR_WIDTH], ~wptr[ADDR_WIDTH-1], wptr[ADDR_WIDTH-2:0]});

endmodule
```
