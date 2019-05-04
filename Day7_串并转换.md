# Verilog没有葵花宝典_20190430

- 1. 复习verilog语法【选做题】
  - 文件操作fopen fdisplay fwrite fclose
  - 生成随机数 random
  - 初始化 readmemh readmemb
  - finish stop
  
- 2. 用verilog实现串并变换。

```verilog
  input [3:0] data_in;
  output [3:0] data_out;
  input [1:0] mode;
  input clk;
  input rst_n;

  mode 0 ：串行输入data_in[0]，并行输出data_out[3:0]
  mode 1 ：并行输入data_in[3:0]，串行输出data_out[0]
  mode 2 ：并行输入data_in[3:0]，并行输出data_out[3:0]，延迟1个时钟周期
  mode 3 ：并行输入data_in[3:0]，并行反序输出data_out[3:0]，延迟1个时钟周期并且交换bit顺序
  data_out[3]=data_in[0]; 
  data_out[2]=data_in[1]
  data_out[1]=data_in[2]
  data_out[0]=data_in[3]
```

  - 附加要求【选做】
  - 将输入输出的位宽做成参数化

- 3. 记录一下第2题中用到的工具，包括工具版本，操作步骤或命令选项，遇到的错误，提示信息等。比较一下，与昨天的记录有何相同，有何不同。

# 打卡day7

#### 1. 复习verilog语法【选做题】

##### 1.1 文件操作fopen fdisplay fwrite fclose

```verilog
handlel = $fopen("file.txt");//打开文件
$fdisplay(handlel,"%d",rand_num);//写数据
$fwrite(handlel,"%d",reg1) ;//写入文件
$fclose(handlel);//关闭文件
```

##### 1.2 生成随机数 random

```verilog
$random函数被调用时返回一个32bit的随机数，它是一个带符号的整形数
rand = $random % 60;//给出了一个范围在－59到59之间的随机数。
rand = {$random} % 60;//通过位并接操作产生一个值在0到59之间的数。
rand = min+{$random} % (max-min+1);//产生一个在min, max之间随机数的例子。
```

##### 1.3 初始化 readmemh readmemb

```verilog
$readmemb("<数据文件名>",<存储器名>);
$readmemb("<数据文件名>",<存储器名>,<起始地址>);
$readmemb("<数据文件名>",<存储器名>,<起始地址>,<终止地址>);

$readmemh("<数据文件名>",<存储器名>);
$readmemh("<数据文件名>",<存储器名>,<起始地址>);
$readmemh("<数据文件名>",<存储器名>,<起始地址>,<终止地址>);

$readmemb中要求数据必须为二进制，$readmemh要求数据必须为十六进制
```

##### 1.4 finish stop

- $stop：用于在仿真时，暂停仿真。运行到$stop的时候，仿真会暂停；此时可以在命令行输入run继续运行仿真。
- $finish：仿真停止。运行到$finish的时候，仿真停止退出，此时不可以再继续运行

#### 2. 用verilog实现串并变换。

- 没研究出来，粘贴隔壁同学的，以后再好好学习。
[https://blog.csdn.net/Reborn_Lee/article/details/89707184](https://blog.csdn.net/Reborn_Lee/article/details/89707184)

#### 3. 记录一下第2题中用到的工具，包括工具版本，操作步骤或命令选项，遇到的错误，提示信息等。比较一下，与昨天的记录有何相同，有何不同。

同上

