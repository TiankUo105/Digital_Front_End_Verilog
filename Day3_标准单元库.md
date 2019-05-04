# Verilog没有葵花宝典_20190424

- 1. 了解目录结构：与前端相关的比如文档(doc)，仿真模型(verilog/vhdl)，标准单元库(synopsys/symbols)
- 2. 阅读文档transition time, propagation delay等参数的定义
- 3. 阅读文档Power Dissipation/Calculation的描述
- 4. 阅读文档Delay calculation的描述
- 5. 提供了哪些类型的cell？
- 6. Verilog文件中包含了哪些信息？

# 打卡day3

#### 背景了解

- 工艺库文件：是指将RTL级的HDL描述映射到门级时所需的标准单元综合库。它是由芯片制造商（Foundry）提供的，包含了物理信息的单元模型。通常芯片制造商提供的基本电路单元有：与非，或非，非门等基本的逻辑单元；还有选择器，与或非，或与非等较复杂的组合逻辑，以及锁存器（LATCH）和触发器（DFF）等时序逻辑单元。DC用到的工艺库是.db或者是.lib格式的，其中.lib格式的文件是可读的，通过此文件可以了解库的详细信息，比如说工作电压，操作温度，工艺偏差等。.db格式的库是二进制的，不可读。

#### 1. 了解目录结构：与前端相关的比如文档(doc)，仿真模型(verilog/vhdl)，标准单元库(synopsys/symbols)

- 文档中有三个文件夹，分别是FE/FX2/FX8，每太搞懂什么区别。每个文件夹里面都有一个aci文件和三个README，可能是对应三种库的介绍。
- FE:最复杂的一个文件，包括文档（doc）仿真模型（verilog/vhdl）和标准单元库（synopsys/symbols）。文档介绍了库中各个cell的逻辑和参数，仿真模型中对应各个cell的硬件化语言，用来仿真，里面有一些工艺参数设计？标准单元库就是DC用到的工艺库，了解工艺相关参数。
- FX2:出现了一堆cdB文件和pcdb文件，好像介绍了接口的电容；
- FX8:tetramax可以在最短的时间内,生成具有最高故障覆盖率的最小的测试向量集。TetraMAX支持全扫描、或不完全扫描设计,同时提供故障仿真和分析能力。

#### 2. 阅读文档transition time, propagation delay等参数的定义

![propagation_delay](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day3/propagation_delay.png)

![transition_time](https://github.com/TiankUo105/Digital_Front_End_Verilog/blob/master/picture/day3/transition_time.png)

#### 3. 阅读文档Power Dissipation/Calculation的描述

- 功耗仿真相关

#### 4. 阅读文档Delay calculation的描述

- 延时相关

#### 5. 提供了哪些类型的cell？

- 各种基本的逻辑运算模型，不知道怎么归类。

#### 6.  Verilog文件中包含了哪些信息？

- 包含cell说明，接口，组成逻辑和spec参数值；
