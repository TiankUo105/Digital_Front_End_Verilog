# Verilog没有葵花宝典_20190426

- 1. dff和latch有什么区别。
- 2. 什么是同步电路和异步电路。
- 3. 什么是setup time和 hold time。
- 4. 设计一个101序列检测器。要画出状态转移图，写verilog，并仿真测试。

# 打卡day5

#### 1. dff和latch有什么区别。

latch（锁存器）与 DFF（D触发器）的区别：
- latch由电平触发，非同步控制。在使能信号有效时latch相当于通路，在使能信号无效时latch保持输出状态。DFF由时钟沿触发，同步控制。
- latch容易产生毛刺（glitch），DFF则不易产生毛刺。
- 如果使用门电路来搭建latch和DFF，则latch消耗的门资源比DFF要少，这是latch比DFF优越的地方。所以，在ASIC中使用latch的集成度比DFF高，但在FPGA中正好相反，因为FPGA中没有标准的latch单元，但有DFF单元，一个LATCH需要多个LE才能实现。
- latch将静态时序分析变得极为复杂。
- 一般的设计规则是：在绝大多数设计中避免产生latch。它会让您设计的时序完蛋，并且它的隐蔽性很强，非老手不能查出。latch最大的危害在于不能过滤毛刺。这对于下一级电路是极其危险的。所以，只要能用D触发器的地方，就不用latch。
- 有些地方没有时钟，也只能用latch了。比如现在用一个clk接到latch的使能端(假设是高电平使能),这样需要的setup时间，就是数据在时钟的下降沿之前需要的时间，但是如果是一个DFF，那么setup时间就是在时钟的上升沿需要的时间。这就说明如果数据晚于控制信号的情况下，只能用latch,这种情况就是，前面所提到的latch timing borrow。基本上相当于借了一个高电平时间。也就是说，latch借的时间也是有限的。

[https://blog.51cto.com/yuqix/214861](https://blog.51cto.com/yuqix/214861)

[https://www.cnblogs.com/zeushuang/archive/2012/08/09/2630385.html](https://www.cnblogs.com/zeushuang/archive/2012/08/09/2630385.html)

#### 2. 什么是同步电路和异步电路。

- 同步逻辑是时钟之间有固定的因果关系。异步逻辑是各时钟之间没有固定的因果关系。 
- 电路设计可分类为同步电路和异步电路设计。同步电路利用时钟脉冲使其子系统同步运作，而异步电路不使用时钟脉冲做同步，其子系统是使用特殊的“开始”和 “完成”信号使之同步。由于异步电路具有下列优点--无时钟歪斜问题、低电源消耗、平均效能而非最差效能、模块性、可组合和可复用性--因此近年来对异步 电路研究增加快速，论文发表数以倍增，而Intel Pentium 4处理器设计，也开始采用异步电路设计。
- 异步电路主要是组合逻辑电路，用于产生地址译码器、FIFO或RAM的读写控制信号脉冲，其逻辑输出与任何时钟信号都没有关系，译码输出产生的毛刺通常是 可以监控的。同步电路是由时序电路(寄存器和各种触发器)和组合逻辑电路构成的电路，其所有操作都是在严格的时钟控制下完成的。这些时序电路共享同一个时 钟CLK，而所有的状态变化都是在时钟的上升沿(或下降沿)完成的。

#### 3. 什么是setup time和 hold time。

- Setup/hold time 是测试芯片对输入信号和时钟信号之间的时间要求。建立时间是指触发器的时钟信号上升沿到来以前，数据稳定不变的时间。输入信号应提前时钟上升沿（如上升沿 有效）T时间到达芯片，这个T就是建立时间-Setup time.如不满足setup time,这个数据就不能被这一时钟打入触发器，只有在下一个时钟上升沿，数据才能被打入触发器。保持时间是指触发器的时钟信号上升沿到来以后，数据稳定 不变的时间。如果hold time不够，数据同样不能被打入触发器。
- 建立时间(Setup Time)和保持时间（Hold time）。建立时间是指在时钟边沿前，数据信号需要保持不变的时间。保持时间是指时钟跳变边沿后数据信号需要保持不变的时间。如果不满足建立和保持时间 的话，那么DFF将不能正确地采样到数据，将会出现stability的情况。如果数据信号在时钟沿触发前后持续的时间均超过建立和保持时间，那么超过量 就分别被称为建立时间裕量和保持时间裕量。

####4. 设计一个101序列检测器。要画出状态转移图，写verilog，并仿真测试。

![101](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day5/101.png)

```verilog
module detector(
 input reset,
 input clk,
 input ipt,
 output reg result
 );
 parameter [1:0] s0 = 2'b00, s1 = 2'b01, s2 = 2'b10;
 reg[1:0] state;
 always@(posedge clk)
 begin
  if(reset)
  begin
   state <= s0;
   result <= 0;
  end
  else
  begin
   case(state)
    s0:
    begin
     if(ipt==0)
     begin
      state <= s0;
      result <= 0;
     end
     else
     begin
      state <= s1;
      result <= 0;
     end
    end
    s1:
    begin
     if(ipt==0)
     begin
      state <= s2;
      result <= 0;
     end
     else
     begin
      state <= s1;
      result <= 0;
     end
    end
    s2:
    begin
     if(ipt==0)
     begin
      state <= s0;
      result <= 0;
     end
     else
     begin
      state <= s0;
      result <= 1;
     end
    end
    default:
    begin
     state <= s0;
     result <= 0;
    end
   endcase
  end
 end
endmodule
```

```verilog
`timescale 1ns/1ns
`define halfperiod 20
module t;
reg clk,reset;
reg [23:0]data;
wire ipt,result;
assign ipt = data[23];
initial
  begin
    clk = 0;
    reset = 1;
    #2 reset = 0;
    #30 reset = 1;
    data = 20'b1100_1001_0000_1001_0100;   //data为24位，但只给了20位，前面的会自动补零。
    #(`halfperiod * 1000)$stop;
  end
always #(`halfperiod) clk = ~clk;
always @(posedge clk)
         #2 data = {data[22:0],data[23]};
detector m(.ipt(ipt), .clk(clk), .reset(reset), .result(result) );
endmodule
```
