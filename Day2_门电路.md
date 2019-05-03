# Verilog没有葵花宝典_20190423

- 1. 画一下电路图：CMOS反相器、与非门、或非门、三态输出门、漏极开路门。
- 2. 解释一下Vih，Vil，Vol，Voh，Vt，Iddq
- 3. CMOS反相器的速度与哪些因素有关？什么是转换时间（transition time）和传播延迟（propagation delay）？
- 4. CMOS反相器的功耗主要包括哪几部分？分别与哪些因素相关？
- 5. 什么是latch-up(闩锁效应)？
- 6. 相同面积的cmos与非门和或非门哪个更快？

# 打卡day2

#### 1. 画一下电路图：CMOS反相器、与非门、或非门、三态输出门、漏极开路门。

- CMOS反相器实现了逻辑非的功能。

![CMOS反相器](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/cmos反相器.png)

- 与非门是与门和非门的结合，先进行与运算，再进行非运算。

![与非门](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/与非门.png)

- 或非门是或门和非门的结合，先进行或运算，再进行非运算。

![或非门](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/或非门.png)

- 三态输出门可以输出高阻态以及0和1，使能决定是否输出高阻态。

![三态输出门](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/三态输出门.png)

- CMOS OD门，可用来“线与”。
![漏极开路门](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/漏极开路门.png)

[OD门需要再深入学习一下](http://www.elecfans.com/baike/bandaoti/jichuzhishi/20100304178298.html)

#### 2. 解释一下Vih，Vil，Vol，Voh，Vt，Iddq

![电压传输特性曲线vtc](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/电压传输特性曲线vtc.png)

- vt：mos管的阈值电压。
- iddq：电流IDDQ是指当CMOS集成电路中的所有管子都处于静止状态时的电源总电流。

#### 3. CMOS反相器的速度与哪些因素有关？什么是转换时间（transition time）和传播延迟（propagation delay）？

- CMOS反相器的速度与哪些因素有关：

![反相器速度](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/反相器速度.png)

- 转换时间（transition time）：上升时间：从10%Vdd上升到90%Vdd的时间，下降时间L从90%Vdd下降到10%dd的时间。上升时间和下降时间统称为Transition Time，也有定义为20%到80%。

![转换时间](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/转换时间.png)

- 传播延迟（propagation delay）：在输入信号变化到50%Vdd到输出信号变化到50%Vdd之间的时间。

![传播延迟](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day2/传播延迟.png)

#### 4. CMOS反相器的功耗主要包括哪几部分？分别与哪些因素相关？

- 包括动态功耗PD和静态功耗PS。
- 动态功耗包括开关瞬态电流造成的功耗PA，负载电容的充电和放电造成的功耗PT。
- 静态功耗是反向漏电流造成的功耗。

#### 5. 什么是latch-up(闩锁效应)？

- 闩锁效应（Latch-up）是CMOS集成电路中一个重要的问题，这种问题会导致芯片功能的混乱或者电路直接无法工作甚至烧毁。指在CMOS晶片中，在电源和地线之间由于寄生的PNP和NPN双极性BJT互相影响而产生的一低阻抗通路，它的存在会使电源和地线之间产生大电流。随着IC制造工艺的发展，封装密度和集成度越来越高，产生latch-up的可能性越来越大，防范latch up是IC layout的最重要措施之一。

#### 6. 相同面积的cmos与非门和或非门哪个更快？

-PMOS采用空穴导电，NMOS采用电子导电，由于PMOS的载流子的迁移率比NMOS的迁移率小，所以，同样尺寸条件下，PMOS的充电时间要大于NMOS的充电时间长，在互补CMOS电路中，与非门是PMOS管并联，NMOS管串联，而或非门正好相反，所以，同样尺寸条件下，与非门的速度快，所以，在互补CMOS电路中，优先选择与非门。


