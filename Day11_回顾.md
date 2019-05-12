# Verilog没有葵花宝典_20190509

- 1.总结写代码和仿真的操作步骤、遇到的错误和提示信息。有哪些共性？有哪些地方可以自动化？

# 打卡day1

#### 1.总结写代码和仿真的操作步骤、遇到的错误和提示信息。有哪些共性？有哪些地方可以自动化？

- Verilog大学时候学的也不精，几年后实习又粗略写了几个月，现在基本已经快忘的差不多了，通过这两周作业算是磕磕绊绊跟着打酱油，但收获也很大，总结一下现在的写代码和仿真步骤。
  - 代码：第一步拿到题目就是各种搜索，百度/CSDN/星球内的大神们提供的资料，基本上代码还在参考和学习。希望接下来能够是模仿和创造吧。
  - 仿真：现在用的是EDA Playground，里面又VCS 2014.10编译器和EPWave用来波形仿真。
  - testbench：基本上有一个套路，没太大问题的话下面这个模板就可以用。
  ~~~verilog
  // Code your testbench here
// or browse Examples
`timescale 1ns/1ns
module XX_tb();
  reg clk,
      rst_n;

  wire xx_out;

  initial begin
    clk = 0;
    forever #21 clk = ~clk;//设定输入的时钟
  end
  
  //reset及一些其他需要输入的信号
  initial begin
    rst_n = 1'b0;
    #42 rst_n = 1'b1;
    #3_000_00
    $finish;//注意这里一定要又finish，不然EPWare停不下来
  end
  
  //这里和虚拟机环境不同，需要自己写
  initial begin
    $dumpfile("tb.vcd");
    $dumpvars();
  end
  
  //这里XXX名字要和module对应
  XXX XXX_t
    (// Outputs
      .XXX_out    (XXX_out),
     // Inputs
     .clk     (clk),
      .rst_n     (rst_n));

  
endmodule // tb
  ~~~
