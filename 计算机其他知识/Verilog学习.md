<center><font size="7" face="黑体">Verilog学习</font></center>


# 一、数字逻辑回顾

***

## 1. 组合电路与时序电路

- 用电路逻辑输出值区分

  > <u>输出值</u>只与当前**输入值**有关的，就是组合电路

  > <u>输出值</u>与当前**输入值**和**电路当前状态**（所以就需要<u>保存当前状态</u>的东西）都有关的，就是时序电路

***

## 2. 逻辑值

- 1个bit有4种状态

  > 1'b1, 1'b0, 1'bx, 1'bz

![image-20211031144543006](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031144543006.png)

<img src="C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031144653220.png" alt="image-20211031144653220" style="zoom:150%;" />

***

# 二、 Verilog

***

## （一）概述

### 1. 什么是Verilog

- 硬件**描述语言**，<u>描述</u>数字电路的功能

  > 不是设计语言
  >
  > 是工程师先设计硬件结构，再用HDL描述

***

### 2. Hello World

```verilog
module hello ();
    initial begin
        $display("Hello World.\n");
    end
endmodule
```

***

### 3. 可综合 & 不可综合

- 可综合描述：

  综合tool能把Verilog描述转化(Compile)为基本的数字电路底层cell的描述

  <img src="C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031150110124.png" alt="image-20211031150110124" style="zoom:75%;" />

- 不可综合描述

  综合tool不能把Verilog描述转化为基本的数字电路底层cell的描述

  > 比如那个Hello World就不可综合

***

### 4. Verilog设计仿真与实现

![image-20211031150353104](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031150353104.png)

***

### 5. 数字电路设计方法

![image-20211031151457557](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031151457557.png)

![image-20211031151523699](C:\Users\不怕晒的铃铛\Desktop\学校课程相关\作业\马原\image-20211031151523699.png)

要注意硬件描述语言的**并行性**

> 这也是因为底层硬件具有<u>并行性</u>

![image-20211031151952383](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031151952383.png)

***

## （二）Verilog基本语法

### 1. 标识符

![image-20211031152442900](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031152442900.png)

***

### 2. 数据物理类型

#### 	(1). 线型

![image-20211031153837916](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031153837916.png)

> wire型几乎都是描述组合逻辑

> 两个线型的典型运用

![image-20211103212226191](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211103212226191.png)

![image-20211103212235089](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211103212235089.png)

![image-20211103213428512](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211103213428512.png)

![image-20211103213450470](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211103213450470.png)

#### 	(2). 寄存器类型

![image-20211031154028893](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031154028893.png)

> initial通常是不可综合的，
>
> always通常是可综合的

![image-20211031154601416](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031154601416.png)

#### 	(3). 一维标量类型——常量

![image-20211031155326522](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031155326522.png)

#### 	(4). 一维标量类型——变量

![image-20211031155615196](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031155615196.png)

#### 	(5). 多维标量类型——变量

![image-20211031160008397](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031160008397.png)

```verilog
array_0[0] = array_1[255][7:4];
//这个[7:4]是在按位赋值
```

***

### 3. 运算符

#### 	(1). 逻辑运算符

![image-20211031162730811](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031162730811.png)

![image-20211031163231869](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031163231869.png)

> 按位：输出与输入位数相同
>
> 整体：只有一位输出	

![image-20211101094309926](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211101094309926.png)

#### 	(2). bit选择与拼接

![image-20211031164041984](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031164041984.png)

>  +:的意思是从...开始，一共取...位
>
> 这其实很符合实际，选择/拼接位只需要拉线即可

#### 	(3). 运算符与位宽

![image-20211031165113367](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031165113367.png)

> 一般情况下，每两个数相加减就要扩一个bit

![image-20211031192714871](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031192714871.png)

- 也可以用在等号左侧

![image-20211101094929968](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211101094929968.png)

![image-20211101095358359](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211101095358359.png)

- 注意写法

![image-20211101095757436](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211101095757436.png)

- 可以用于扩展符号位

![image-20211101100721323](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211101100721323.png)

- 与逻辑运算符结合，极大地简化代码

![image-20211101101125733](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211101101125733.png)

![image-20211101101136104](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211101101136104.png)

#### 	(4). 多重条件运算

![image-20211031165814070](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031165814070.png)

![image-20211031171105019](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031171105019.png)

> if-else带优先级，case不带优先级

***

## （三）Verilog功能模块构建

### 1. module组成部分

- 模块名
- 输入输出端口定义
- 内部信号定义
- 实例化其他模块
  - 起一个自己的实例名
  - 给所用模块的端口接线

***

### 2. 模块举例——组合逻辑

#### 	(1). 多路选择器

![image-20211031180004661](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031180004661.png)

#### 	(2). 3-8译码器

![image-20211031181916719](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031181916719.png)

> 巧妙利用16进制

***

### 3. 模块举例——同步复位与异步复位

![image-20211031193242261](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031193242261.png)

## （四）Verilog语句执行顺序

### 1. 模块的执行时机

![image-20211031225719409](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031225719409.png)

> *initial*：仿真**一开始**就执行
>
> *assign*：**输入有变化**的时候就去执行一次
>
> *always*：
>
> > 电平触发：<u>敏感列表</u>中的**信号**变化
> >
> > 边沿触发：上升沿/下降沿到来

***

### 2. 赋值语句

#### 	(1). 非阻塞赋值

![image-20211031230137191](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031230137191.png)

> 赋值语句先后顺序都无所谓了

#### 	(2). 阻塞赋值

![image-20211031231914705](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031231914705.png)

> 也能实现和非阻塞赋值一样的效果，但调换顺序就会出问题

![image-20211031232309228](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031232309228.png)

> 所以，不要这么写

#### (3). 代码编写建议

![image-20211031232741234](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031232741234.png)

> 不允许多驱动（左图a~0~_mac就被多驱动了）

![image-20211031233226894](C:\Users\不怕晒的铃铛\AppData\Roaming\Typora\typora-user-images\image-20211031233226894.png)

