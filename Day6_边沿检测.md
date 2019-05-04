# Verilog没有葵花宝典_20190429

- 1. 复习verilog语法【选做题】
  - reg和wire的区别
  - 阻塞赋值与非阻塞赋值的区别
  - parameter与define的区别
  - task与function的区别
- 2. 用verilog实现边沿检测电路：上升沿，下降沿，双沿(上升或下降沿)。
- 3. 记录一下第2题中用到的工具，包括工具版本，操作步骤或命令选项，遇到的错误，提示信息等。


# 打卡day6

#### 1. 复习verilog语法【选做题】

- reg和wire的区别
  - 从仿真的角度来说，HDL语言面对的是编译器（如Modelsim等），相当于软件思路。这时：
    - wire对应于连续赋值，如assign
    - reg对应于过程赋值，如always，initial
  - 从综合的角度来说，HDL语言面对的是综合器（如DC等），要从电路的角度来考虑。这时：
    - wire型的变量综合出来一般是一根导线；
    - reg变量在always块中有两种情况：
      - always后的敏感表中是（a or b or c）形式的，也就是不带时钟边沿的，综合出来还是组合逻辑
      - always后的敏感表中是（posedge clk）形式的，也就是带边沿的，综合出来一般是时序逻辑，会包含触发器（Flip－Flop）
      
[https://www.eefocus.com/wzl334194017/blog/16-09/391165_67613.html](https://www.eefocus.com/wzl334194017/blog/16-09/391165_67613.html) 
 
- 阻塞赋值与非阻塞赋值的区别
  - 阻塞：在本语句中“右式计算”和“左式更新”完全完成之后，才开始执行下一条语句；
  - 非阻塞：当前语句的执行不会阻塞下一语句的执行。
  
[https://www.cnblogs.com/byilrq/p/6040611.html](https://www.cnblogs.com/byilrq/p/6040611.html)
  
- parameter与define的区别

[http://bbs.elecfans.com/jishu_332513_1_1.html](http://bbs.elecfans.com/jishu_332513_1_1.html)
  
- task与function的区别

[https://blog.csdn.net/kobesdu/article/details/39080571](https://blog.csdn.net/kobesdu/article/details/39080571)

#### 2. 用verilog实现边沿检测电路：上升沿，下降沿，双沿(上升或下降沿)。

```verilog
// Code your design here
module edge_detect(
  input clk,
  input rst_n, 
  input data_in, 
  output raising_edge_detect, 
  output falling_edge_detect, 
  output double_edge_detect);
  
  reg data_in_d1;
  reg data_in_d2;
  always @ (posedge clk,negedge rst_n)
 begin
      if(!rst_n)
     begin 
          data_in_d1 <= 1'b0; data_in_d2 <= 1'b0; 
        end 
      else
     begin 
          data_in_d1 <= data_in; data_in_d2 <=  data_in_d1;
        end 
    end 
  assign raising_edge_detect = data_in_d1  & (~data_in_d2);//上升沿
  assign falling_edge_detect = ~data_in_d1 &  data_in_d2;//下降沿
  assign double_edge_detect  = data_in_d1 ^ data_in_d2;//双边沿
endmodule
```

```verilog
// Code your testbench here
// or browse Examples
`timescale 1ns/1ps
module tb();
  reg clk,
      rst_n,
      data_in;
  wire raising_edge_detect;
  wire falling_edge_detect;
  wire double_edge_detect;
  initial begin
    clk = 0;
    forever #10 clk = ~clk;
  end
  
  initial begin
    rst_n = 1'b0;
    #22 rst_n = 1'b1;
  end
  
  initial begin
    repeat(100)begin
      @(negedge clk)
 data_in = {$random};
    end
    $finish;
  end
  
  initial begin
    $dumpfile("edge_detect_tb.vcd");
    $dumpvars();
  end
  
  edge_detect edge_detect_i
    (// Outputs
     .raising_edge_detect    (raising_edge_detect),
     .falling_edge_detect    (falling_edge_detect),
     .double_edge_detect    (double_edge_detect),
     // Inputs
     .clk     (clk),
     .rst_n     (rst_n),
     .data_in    (data_in));
  
endmodule // tb
```

#### 3. 记录一下第2题中用到的工具，包括工具版本，操作步骤或命令选项，遇到的错误，提示信息等

edaplayground：VCS+EPWave

![边沿检测sim](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day6/边沿检测sim.png)


