# Verilog没有葵花宝典打卡day1

#### 1. bit, byte, word, dword, qword的区别

- bit：位或比特，1bit=1位二进制；
- byte：字节，1byte=8位二进制；
- word：字，1word=2byte,16位二进制；
- dword：double word双字，1dword=2word，32位二进制;
- qword：quad word四字，1qword=4word，64位二进制；

#### 2. 什么是原码，反码，补码，符号-数值码。以8bit为例，给出各自表示的数值范围。

原码，反码，补码是不同的编码方式。在计算机中，数值一律用补码来存储，使用补码可以将符号位和其他位统一处理，并且减法也可以按照加法来处理。
- 原码：左边第一位表示符号（0为正，1为负），其余位表示数值。8bit数值范围是-127~+127。
- 反码：如果是正数与原码一样，如果是负数符号位不变，其余位是原码各个位取反。8bit数值范围是-127~+127。
- 补码：如果是正数与原码一样，如果是负数，在反码的基础上加一。8bit数值范围是-128~+127。

#### 3. 十进制转换为二进制编码： 127， （-127），127.375，（-127.375）

补码：total总位宽 11bit，fix定点精度 3bit，第一位为符号为0+，1-
- 127：0111_1111_000；
- (-127)：1000_0001_000;
- 127.375：0111_1111_011;
- (-127.375)：1000_0000_101;

#### 4. 设计BCD译码器，输入0~9。采用verilog描述并画出门级电路图。

```verilog
module segment8(
    input [3:0] segin,
    output reg [7:0] segout);
   
   always @(seg_in)
     case(seg_in)
         //gfedcba 共阳极
         4'b0000:segout=7'b1000000;//display digital 0(40H)
         4'b0001:segout=7'b1111001;//display digital 1(79H)
         4'b0010:segout=7'b0100100;//display digital 2(24H)
         4'b0011:segout=7'b0110000;//display digital 3(30H)
         4'b0100:segout=7'b0011001;//display digital 4(19H)
         4'b0101:segout=7'b0010010;//display digital 5(12H)
         4'b0110:segout=7'b0000010;//display digital 6(02H)
         4'b0111:segout=7'b1111000;//display digital 7(78H)
         4'b1000:segout=7'b0000000;//display digital 8(00H)
         4'b1001:segout=7'b0010000;//display digital 9(10H)
         4'b1010:segout=7'b0001000;//display digital A(08H)
         4'b1011:segout=7'b0000011;//display digital B(03H)
         4'b1100:segout=7'b0100111;//display digital C(27H)
         4'b1101:segout=7'b1000001;//display digital d(21H)
         4'b1110:segout=7'b0000110;//display digital E(06H)
         4'b1111:segout=7'b0001110;//display digital F(0EH)
         default:segout=7'b0100011;//display digital 0(23H)
     endcase
endmodule
```

![test](https://qqadapt.qpic.cn/txdocpic/0/b8061fb0e0a1f6445b83103a75544400/0)

[https://blog.csdn.net/a1260157543/article/details/49227639](https://blog.csdn.net/a1260157543/article/details/49227639) 
