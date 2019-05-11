# Verilog没有葵花宝典_20190508
- 1. 用verilog实现PWM控制呼吸灯。呼吸周期2秒：1秒逐渐变亮，1秒逐渐变暗。系统时钟24MHz，pwm周期1ms，精度1us。

# 打卡day10
#### 1. 用verilog实现PWM控制呼吸灯。呼吸周期2秒：1秒逐渐变亮，1秒逐渐变暗。系统时钟24MHz，pwm周期1ms，精度1us。
- 二话不说先上参考[http://www.stepfpga.com/doc/%E5%91%BC%E5%90%B8%E7%81%AF%7D](http://www.stepfpga.com/doc/%E5%91%BC%E5%90%B8%E7%81%AF%7D)
- 学习了大神同学的代码，跑一下仿真。[Verilog没有葵花宝典打卡day10.md](https://github.com/NingHeChuan/Digital_Front_End_Verilog/blob/master/Verilog%E6%B2%A1%E6%9C%89%E8%91%B5%E8%8A%B1%E5%AE%9D%E5%85%B8/Verilog%E6%B2%A1%E6%9C%89%E8%91%B5%E8%8A%B1%E5%AE%9D%E5%85%B8%E6%89%93%E5%8D%A1day10.md)

~~~verilog
// Code your design here
module Breath_LED(
    input           clk,    //24Mhz
    input           rst_n,
    output          led_out
);

parameter   DELAY24 = 24;
//parameter   DELAY1000 = 1000;
parameter   DELAY1000 = 10;//just test


wire            delay_1us;
wire            delay_1ms;
wire            delay_1s;
reg             pwm;
reg     [7:0]   cnt1;
reg     [10:0]  cnt2;
reg     [10:0]  cnt3;
reg             display_state;

//延时1us
always @(posedge clk or negedge rst_n)begin
    if(!rst_n)
        cnt1 <= 6'b0;
    else if(cnt1 == DELAY24 - 1'b1)
        cnt1 <= 6'b0;
    else 
        cnt1 <= cnt1 + 1'b1;
end

assign delay_1us = (cnt1 == DELAY24 - 1'b1)? 1'b1:1'b0;

//延时1ms
always @(posedge clk or negedge rst_n)begin
    if(!rst_n)
        cnt2 <= 10'b0;
    else if(delay_1us == 1'b1)begin
        if(cnt2 == DELAY1000 - 1'b1)    
            cnt2 <= 10'b0;
        else 
            cnt2 <= cnt2 + 1'b1;
    end
    else 
        cnt2 <= cnt2;
end
assign delay_1ms = ((delay_1us == 1'b1) && (cnt2 == DELAY1000 - 1'b1))? 1'b1:1'b0;
   
//延时1s
always @(posedge clk or negedge rst_n)begin
    if(!rst_n)
        cnt3 <= 10'b0;
    else if(delay_1ms)
    begin
         if(cnt3 == DELAY1000 - 1'b1)
            cnt3 <= 10'b0;
        else 
            cnt3 <= cnt3 + 1'b1;
     end
    else 
        cnt3 <= cnt3;    
end

assign delay_1s = ((delay_1ms == 1'b1) && (cnt3 == DELAY1000 - 1'b1))? 1'b1:1'b0;

//state change
always @(posedge clk or negedge rst_n)begin
    if(!rst_n)
        display_state <= 1'b0;
    else if(delay_1s)//每一秒切换一次led灯显示状态
        display_state <= ~display_state;
    else 
        display_state <= display_state;
end

//pwm信号的产生
always @(posedge clk or negedge rst_n)begin
    if(!rst_n)
            pwm <= 1'b0;
    else 
        case(display_state)
            1'b0: pwm <= (cnt2 < cnt3)? 1'b1:1'b0;
            1'b1: pwm <= (cnt2 < cnt3)? 1'b0:1'b1;
            default: pwm <= pwm;
        endcase
end

//位拼接使得输出八位led呼吸灯
assign led_out = pwm;

endmodule 
~~~

~~~verilog
// Code your testbench here
// or browse Examples
`timescale 1ns/1ns
module Breath_LED_tb();
  reg clk,
      rst_n;

  wire led_out;

  initial begin
    clk = 0;
    forever #21 clk = ~clk;
  end
  
  initial begin
    rst_n = 1'b0;
    #42 rst_n = 1'b1;
    #1_000_000
    $finish;
  end
  
  
  initial begin
    $dumpfile("tb.vcd");
    $dumpvars();
  end
  
  Breath_LED Breath_LED_t
    (// Outputs
      .led_out    (led_out),
     // Inputs
     .clk     (clk),
      .rst_n     (rst_n));

  
endmodule // tb
~~~
