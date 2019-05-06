# Verilog没有葵花宝典_20190506
- 1. 用verilog实现一个4bit二进制计数器。
  - a) 异步复位
  - b) 同步复位
~~~verilog
input clk, rst_n; 
output [3:0] o_cnt;
~~~
- 2. 用verilog实现4bit约翰逊(Johnson)计数器。
- 3. 用verilog实现4bit环形计数器：复位有效时输出0001，复位释放后依次输出0010，0100，1000，0001，0010...
- 4. 比较一下以上三种计数器的特点。
- 5. 记录1,2,3题目使用的工具，操作步骤，以及出现的错误和提示信息。

# 打卡day8
#### 1. 用verilog实现一个4bit二进制计数器。
  - a) 异步复位
  - b) 同步复位
~~~verilog
input clk, rst_n; 
output [3:0] o_cnt;
~~~

- 计数器科普：计数器是数字系统中用得较多的基本逻辑器件。它不仅能记录输入时钟脉冲的个数，还可以实现分频、定时、产生节拍脉冲和脉冲序列等。例如，计算机中的时序发生器、分频器、指令计数器等都要使用计数器。 
- 计数器种类：计数器的种类很多。按时钟脉冲输入方式的不同，可分为同步计数器和异步计数器；按进位体制的不同，可分为二进制计数器和非二进制计数器；按计数过程中数字增减趋势的不同，可分为加计数器、减计数器和可逆计数器。
- 为什么还需要十进制计数器：二进制计数器具有电路结构简单、运算方便等特点，但是日常生活中我们所接触的大部分都是十进制数，特别是当二进制数的位数较多时，阅读非常困难，还有必要讨论十进制计数器。在十进制计数体制中，每位数都可能是0，1，2，…，9十个数码中的任意一个，且“逢十进一”。根据计数器的构成原理，必须由四个触发器的状态来表示一位十进制数的四位二进制编码。而四位编码总共有十六个状态。所以必须去掉其中的六个状态，至于去掉哪六个状态，可有不同的选择。
- 同步复位和异步复位的区别：
  - [https://blog.csdn.net/chriscb/article/details/77097548](https://blog.csdn.net/chriscb/article/details/77097548)
  - [https://blog.csdn.net/wordwarwordwar/article/details/74091757](https://blog.csdn.net/wordwarwordwar/article/details/74091757)

~~~verilog
// Code your design here
module cnt2s_4bit(
    input clk, rst_n,
    output [3:0] o_cnt
    );
  reg [3:0] cnt;
//异步复位
/* always @ (posedge clk or negedge rst_n) begin
    if ( !rst_n )
        cnt <= 4'b0000;
    else if ( cnt == 4'b1111 )
        cnt <= 4'b0000;
    else
        cnt <= cnt + 1'b1;
end */
//同步复位
always @ (posedge clk) begin
    if ( !rst_n )
        cnt <= 4'b0000;
    else if ( cnt == 4'b1111 )
        cnt <= 4'b0000;
    else
        cnt <= cnt + 1'b1;
end

assign o_cnt = cnt;

endmodule
~~~

~~~verilog
// Code your testbench here
// or browse Examples
`timescale 1ns/1ps
module cnt_tb();
  reg clk,
      rst_n;

  wire [3:0] o_cnt;

  initial begin
    clk = 0;
    forever #2 clk = ~clk;
  end
  
  initial begin
    rst_n = 1'b0;
    #10 rst_n = 1'b1;
    #100 rst_n = 1'b0;
    #40
    $finish;
  end
  
  
  initial begin
    $dumpfile("tb.vcd");
    $dumpvars();
  end
  
  cnt2s_4bit cnt2s_4bit_t
    (// Outputs
      .o_cnt    (o_cnt),
     // Inputs
     .clk     (clk),
      .rst_n     (rst_n));

  
endmodule // tb
~~~


#### 2. 用verilog实现4bit约翰逊(Johnson)计数器。

#### 3. 用verilog实现4bit环形计数器：复位有效时输出0001，复位释放后依次输出0010，0100，1000，0001，0010...

#### 4. 比较一下以上三种计数器的特点。

#### 5. 记录1,2,3题目使用的工具，操作步骤，以及出现的错误和提示信息。
