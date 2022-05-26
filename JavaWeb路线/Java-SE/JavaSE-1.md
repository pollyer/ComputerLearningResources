# JavaSE (上)

---

---

---

## 接收输入

---

---

### 一、步骤

---
1. 创建扫描器对象
2. 接收输入

```java
import java.util.Scanner;
public class KeyInput {
    public static void main(String[] args) {
        //创建一个扫描器对象
        Scanner s = new Scanner(System.in);
        //接收输入
        int i =  s.nextInt();
    }
}
```
> 更具有<u>交互性</u>

---

### 二、类型问题

---

#### （一）输入不匹配的错误

<img src=".\pics\InputMismatch.png" style="zoom:150%;" />

```java
s.nextInt();
//如果输入的不是int类型数字，则会出现运行时异常：InputMismatchExcption
```

#### （二）接收字符串类型

```java
String str = s.next();
```

---

---

## 面向对象理论

---

---

### 一、与面向过程对比

---

#### （一）语言方面

> 注意：以下是比较通俗的理解，但<u>并不完全正确</u>

- `C`：面向过程
- `C++`：半面向过程、半面向对象
- `Java`：面向对象

#### （二）面向过程

- 特点
  - 注重**步骤**
  - 注重**因果关系**
- 缺点
  - 耦合度高
  - 可扩展性差
- 优点
  - 对于小型项目来说效率高，不需要提取对象，可以直接开干！

#### （三）面向对象

- 特点
  - 符合人类思维（这也是面向对象思想成为主流的原因）
  - 将现实世界分割成单元，单元实现成**对象**，然后再给个环境驱动一下，这样协作起来就形成了一个系统
- 优点
  - 耦合度低
  - 可扩展性强

---

### 二、面向对象开发

---

#### （一）三个阶段

1. OOA(*Object-Oriented Analysis*)：面向对象分析
2. OOD(*Object-Oriented Design*)：面向对象设计
3. OOP(*Object-Oriented Programming*)：面向对象编程

> 面向对象思想贯穿整个开发过程

#### （二）三大特征

1. 封装
2. 继承
3. 多态

---

### 三、类和对象的概念

---

#### （一）类

- <u>在现实世界中不存在</u>

- 是人类大脑**思考**、**抽象**、**总结**出来的

- 是一种抽象、一个模板，描述的是事物的**共同特征**

  > “共同特征”包括什么？
  >
  > 状态$\rightarrow$**属性**（名词）
  >
  > 动作$\rightarrow$**方法**（动词）

#### （二）对象

- <u>实际存在</u>的个体
- 通过“类”这个模板创造出来的
- “对象”也叫做“**实例**”

#### （三）关系

- 类$\rightarrow$对象：创建、实例化
- 对象$\rightarrow$类：抽象

---

### 四、Java软件工程师

---

####  （一）作用

- 开发软件，解决现实生活中的问题
- 进行类与对象之间的转化

#### （二）能力

- **观察**+**抽象**能力
- 使用“类”来描述共同特征

---

### 五、类的定义和对象的创建

---

#### （一）定义类

```java
[修饰符列表] class 类名 {
    //类体 = 属性 + 方法
    //属性以变量的形式存在
}
```

> 为什么属性以变量的形式存在？
>
> 因为属性的本质是一种“**数据**”，而数据只能存放在<u>变量</u>中

> 变量的分类：
>
> ​	<u>方法体内</u>声明的变量：**局部变量**
>
> ​	<u>方法体外</u>声明的变量：**成员变量**（属性）

#### （二）创建对象

- 构造方法

  - 语法结构

    ```java
    [修饰符列表] 构造方法名(形式参数列表) {
        构造方法体;
    }
    ```

    > 注意
    >
    > - 简单情况下修饰符列表只写public，不能写public static
    > - 构造方法名与**类名**必须一致
    > - 构造方法不需要指定返回值类型

  - 缺省构造器

  - 实例变量默认值

    - 对于构造方法中没有赋值的实例变量：
      - 优先赋自定义默认值
      - 如果没有自定义默认值，再赋系统默认值

    

---

---

## JVM内存概述

---

---

### 一、JVM与类和对象

---

<img src=".\pics\009-对象和引用.png" style="zoom:150%;" />

#### （一）栈区

- **调用方法**时会在栈区压栈

  > 方法的参数传递问题：
  >
  > <img src=".\pics\methodstack.png" alt="mothod" style="zoom:70%;" />
  >
  > Java中规定，方法调用传递参数时，无论是基本数据类型还是**引用数据类型**，都是***copy***一份

- 定义在方法中的变量是**局部变量**，因而局部变量存储于栈中

  > **作为局部变量引用**本身也是存储在栈区中的

#### （二）堆区

- **new出来的对象**都存储在堆区（==new的作用就是在堆内存中开辟一块空间==）

  > 默认值问题：
  >
  > <img src=".\pics\defaultvalue.png" alt="deva" style="zoom:80%;" />

  > 垃圾回收机制：
  >
  > - Java中的GC主要针对的是堆内存中的垃圾数据
  > - Java中规定程序员无权利直接操作堆内存（只能通过引用变量去间接操作），所以当某个对象没有被任何引用指向时，GC就会考虑释放这个对象的内存

- 对象包括其中的**实例变量**都存储在堆中

  > 注意，类的**静态变量**不是“对象级别”的，是存储在方法区的

#### （三）方法区

- 主要存储**代码片段**

  > 还包括<u>类的信息</u>、<u>字节码信息</u>

- 方法区是最先有数据的，因为类总是最先加载

## 封装

---

---

### 一、什么是封装

---

- 是面向对象的首要特征

- 类对自身**属性**、**方法**的保护

  > 将**属性和行为**作为一个**整体**来表现事物
  >
  > 再加以**权限控制**

---

### 二、封装有什么用

---

- 普遍意义下的作用
  - 保证内部结构的安全
  - 屏蔽复杂，暴露简单
- 代码级别
  - 调用者不能随意修改、访问，数据安全
  - 调用者不用关心具体实现，很方便

---

### 三、怎么进行封装

---

1. 属性**<u>私有化</u>**

   > 使用`private`修饰属性，这样该属性就<u>只能在本类中直接访问</u>了
   >
   > （出了这个类，就不能直接访问了，连查看都不能查看）

2. 对外提供简单的**<u>操作入口</u>**（，并设立“入口关卡”）

   > 两个操作：读、写（get方法和set方法）
   >
   > 这两个方法应该都是**实例方法**，不能带`static`

---

---

## static与this

---

### 一、static

---

#### （一）基本概念

- `static`修饰的都是类相关的、<u>类级别</u>的

  > 类级别的，采用`类名.`的方式去访问/调用

- `static`修饰的变量称为“<u>静态变量</u>”

  > 变量的分类：
  >
  > - 变量根据其声明位置可划分为：
  >   - 在方法体外声明的：**局部变量**
  >   - 在方法体内声明的：**成员变量**（<u>属性</u>）
  >     - 成员变量又可以分为：
  >       - **实例变量**
  >       - **静态变量**

- `static`修饰的方法称为“<u>静态方法</u>”

  > 没有static修饰的方法就是“实例方法”了

#### （二）静态变量

- *基本特点*

  - `static`修饰的成员变量
  - 在<u>**类加载**</u>时初始化，存储在**方法区**

- *什么时候用*

  - 变量是**类相关的**，不随对象的变化而变化
  - <u>可以节省内存开销</u>

- *怎么访问*

  - :star:`类名.静态变量名`

    > 在同一个类中可以省略`类名.`

  - 使用“引用.”的方式也可以

    - 不建议这样，因为容易造成混淆

    - 还可能造成一个“假的空指针异常”

      > 这是因为，在运行时，用来访问类相关的<u>引用</u>会被替换成<u>类名</u>
      >
      > （就算这个引用指向null）

      > 空指针异常的两个条件：
      >
      > - <u>空引用</u>
      > - 访问<u>实例相关</u>的

#### （三）静态方法

- *基本特点*

  - `static`修饰的方法
  - 方法都存储在方法区，当然静态方法也存储在方法区

- *什么时候用*

  - <u>方法描述的行为</u>不一定要由对象触发，<u>行为结果</u>不随对象变化而变化

  - <u>工具类</u>中的<u>工具方法</u>常常是静态方法

    > 不用创建对象就可以调用，<u>方便</u>

  - 还要遵循一个条件：方法不会直接访问<u>实例变量</u>

    > 换句话说，==直接访问实例变量的方法一定是实例方法==

- *怎么调用*

  - :star:`类名.静态方法名(参数)`

    > 在同一个类中可以省略`类名.`

  - 使用“引用.”的方式也可以，但不建议

#### （四）静态代码块

- *基本特点*

  - 语法

    ```java
    static {
        java语句;
        java语句;
        ......
    }
    ```

  - 在**类加载**时执行，并且**只执行一次**

    > 是<u>**在main方法之前**</u>执行

    > :star:怎么导致一个**类的加载**呢？
    >
    > - 用`java`命令**运行**这个类
    > - `Class.forName(String)`方法会导致类加载

  - 一个类中可以编写<u>多个</u>静态代码块，按<u>自上而下的顺序</u>执行

- *作用*

  - 提供一个特殊的时机：**类加载时机**

- *执行顺序问题*

  - 与其他<u>静态代码</u>（静态变量、静态方法）都在**类加载**时执行

    > <u>无法从静态上下文中引用非静态</u>

  - 所有静态相关代码总体上是<u>自上而下的顺序</u>执行的

    > 禁止“<u>非法前向引用</u>”
    >
    > 所以不能在<u>静态代码块</u>中引用在其下面定义的<u>静态变量</u>

    > 但<u>两个静态方法</u>之间是不区分顺序的

#### （$\rightarrow$）实例代码块

- *执行时机*

  - **对象构建时机**

    > 在任何一个构造方法即将执行之前，都会去执行实例代码块中的代码
    >
    > （就算把实例代码块写在构造方法下面了，也会先执行实例代码块）

- *作用*

  - 多个<u>构造方法代码复用</u>

---

### 二、this

---

#### （一）基本点

- `this`*是什么*

  - 一个引用
  - 保存对象自身的地址

- `this`*存储在哪*

  - 存储于堆内存中

  > `this`是在对象“内部”的

#### （二）应用

- *应用范围/什么时候用*

  - `this`只能用于**实例方法**和**构造方法**中，代表当前对象

    > `this.`通常可以省略

  - `this`不能<u>静态方法</u>

    > 因为静态方法的调用不需要对象，而`this`代表“<u>当前对象</u>”，矛盾了
    >
    > （不能从静态上下文中引用非静态 变量）

- *使用方法/怎么用*

  - :star:`this.`访问实例变量（或实例方法）

    - 同一个类中，`this.`一般可以省略

    - 用于区分实例变量与实例方法参数名时`this.`不能省略

      > 这样也可以增强代码可读性

  - :star:`this(参数)`在一个<u>构造方法中</u>调用本类<u>另外一个构造方法</u>

    - 当然，只能在构造方法中这么用，可以实现<u>代码复用</u>
    - 而且，`this(参数)`语句只能是构造方法中**第一个执行的语句**

---

---

## 继承

---

---

### 一、概述

---

#### （一）什么是继承

- <u>基于已有的类创造新的类</u>
- ***is a*** 的关系

> *superclass*（父类、超类、基类）和*subclass*（子类、派生类、扩展类）

#### （二）有什么用

- *基本作用*：实现类的复用（<u>代码复用</u>）
- *重要作用*：有了继承这个特征，才能有方法覆盖和<u>多态</u>

---

### 二、Java中继承的特性

---

#### （一）简单性

- Java**不支持多继承**（Java简单性的体现），但可以有“间接继承”的效果

#### （二）范围

- <u>构造方法无法继承</u>；==`private`修饰的也可以继承==，只不过在子类中无法直接访问

#### （三）根类

- 不写`extends`时会默认继承`Object类`；`Object类`是 Java 语言的根类

  > 就算`extends`的不是`Object类`，最终也会间接继承到`Object类`

#### （四）耦合

- 继承<u>提高了耦合度</u>，父类修改后子类也会受到影响

---

---

## 方法覆盖与多态

---

---

### 一、方法覆盖

---

#### （一）概述

- ***override***、***overwirtre***、方法重写

#### （二）什么时候用

- 子类**继承**父类后，继承的<u>（实例）方法</u>无法满足子类需求时

- **构造方法**<u>不能继承</u>，因而无法覆盖

- **私有方法**不能覆盖

  > 从“方法覆盖”定义的角度来讲：
  >
  > - 子类无权限访问父类的私有方法，谈不上覆盖
  >
  > 从“多态”的角度来讲：
  >
  > - 首先，如果是在这个类外面，根本就没有权限调用，多态的编译阶段根本过不去
  > - 其次，如果是在这个类的内部，有权限去访问私有方法了，能过编译了，那也不能覆盖，因为如果使用父类引用，私有方法会静态绑定到父类的私有方法上，当然，如果使用子类引用（或者匿名），就会去调用子类的同名方法了（前提是有权访问到同名方法）

  > 那私有的方法有什么意义？
  >
  > 留着自己用

- **静态方法**的覆盖无意义

  > <u>方法覆盖要和多态联合起来才有意义</u>
  >
  > 使用父类型引用调用静态方法时，引用会被替换成<u>父类类名</u>（提前静态绑定了），就没有所谓的“多态”了

#### （三）怎么用/方法覆盖的条件

- 有**继承**关系

  > 两个**类**之间有继承关系
  >
  > **返回值**之间要么<u>相同</u>（基本数据类型必须相同），要么也有<u>继承关系</u>（当然这样没多大意义）

- 相同的**方法名**和**参数列表**

  > 但不能是私有方法、静态方法、构造方法

- **访问权限**不能更低

  > 修饰列表中要么都有`static`，要么都没有`static`，不然报错

- 不能抛出宽泛的**编译时异常**

> 回顾Java中方法重载的条件：
>
> - <u>同一个类</u>当中
> - <u>方法名相同</u>
> - <u>参数列表不同</u>（类型、顺序、个数）

---

### 二、多态

---

#### （一）什么是多态

- *重要特征*

  - ==**父类型引用指向子类型对象**==

- *转型*

  > 无论是向下转型还是向上转型，两个类之间都要有**继承**关系
  >
  > （不然会报错：<u>不兼容的类型</u>）

  - **向上转型**(***upcasting***)：子$\rightarrow$父

    - 可以自动转

      > 直接让父类型引用指向子类型对象，可以“自动地”向上转型了

  - **向下转型**(***downcasting***)：父$\rightarrow$子

    - 要加强制类型转换符

      > 向下转型一般是为了调用<u>子类特有的方法</u>（或者访问子类特有的属性）

- *多态的概念：多种形态*

  - **编译阶段**一种形态

    > 编译器**静态绑定**：
    >
    > 检查语法时，会去父类的字节码文件中找对应的方法，找到了就会进行静态绑定

  - **运行阶段**另一种形态

    > 实际上在堆内存中创建的对象是子类对象，所以在运行阶段会去**动态绑定**并执行子类对象的方法

#### （二）怎么使用多态

- *利用“向上转型”*

  - 让<u>父类型引用</u>指向<u>子类型对象</u>
  - 然后去调用<u>被覆盖的方法</u>即可

- *利用“向下转型”*

  - 先用`instanceof`运算符判断**类型是否兼容**

    > 必要时使用`instanceof`是很重要的，这也是 Java 规范中要求的

    > 如果类型不兼容，可能绕过编译器的检查，但运行时会抛出<u>`ClassCastException`异常</u>

  - 再去用`(子类型)父类型引用`去**向下转型**

  - 然后就可以进行**子类特有行为**了

#### （三）多态的作用

- 符合**开闭原则**(***OCP***)：<u>对扩展开放</u>，<u>对修改关闭</u>

  > 在开发项目时不仅仅要满足客户需求，还要考虑软件的可扩展性

- 可以从多个相似类中<u>提取出父类</u>，更符合**面向对象编程**的思维

  > 其实也是**面向抽象编程**，从而提高了扩展力

- ==降低耦合度，提高扩展力==

---

---

##  super

---

---

### 一、super的概念

---

#### （一）文字描述

- `super`代表当前对象的“==父类型特征==”
- `super`不是引用，不保存地址，不能单独使用`super`

#### （二）内存图描述

<img src=".\pics\021-super的原理.png" alt="super" style="zoom:150%;" />

---

### 二、super的应用

---

#### （一）在哪里用

- `super`只能在构造方法、实例方法中出现
- `super`不能用于<u>静态方法</u>

#### （二）怎么用

- *调用父类构造方法*

  - `super(参数)`，调用父类构造方法

    > 在创建子类对象时，==初始化父类型特征==
    >
    > （无论怎样折腾，<u>父类的构造方法</u>都是必然执行的）
    >
    > （所以，构造方法的执行并不一定是为了<u>创建对象</u>，还可能是为了<u>初始化父类型特征</u>）

    > 和`this(参数)`很像，只能是构造方法中第一个执行的语句，与`this(参数)`不共存；
    >
    > 当构造方法中既没有写明`this(参数)`，又没有写明`super(参数)`时，==缺省情况下会先执行super();==

    > 常用于<u>在子类构造方法中给父类**私有属性**赋值</u>

- *访问父类属性（/方法）*

  - 子类**没有同名**属性（/方法）

    - 对于父类的**私有**属性（/方法）
      - `this.`和`super.`都**无权访问**
    - 对于父类的**非私有**属性（/方法）
      - `this.`和`super.`的**<u>效果相同</u>**

  - :star:子类有**同名**属性（/方法）

    - 是父类的**私有**属性（/方法）

      - `super.`还是**无权访问**，`this.`访问不到

    - :star:是父类的**非私有**属性（/方法）

      - :star:`super.`和`this.`会**分别访问**到父类和子类的属性（/方法）

      > 这时`super.`和`this.`就<u>有区分</u>了，就<u>不能随意省略</u>了
      
      > 这里也为之后说的“方法覆盖”埋下了伏笔。事实上，子类同名方法覆盖了父类方法后，通过`this`访问的当然是子类方法，但仍然可以通过`super`访问到被覆盖的方法
      >
      > > 当然，`abstract`的方法，子类是无法通过`super`调用的。父类(一定是抽象类或接口了)自己当然是可以调用的

---

---

## final

---

---

### 一、概述

---

- 最终的、不可变的
- 可以用来修饰变量、方法、类

---

### 二、应用

---

#### （一）final修饰类

- `final`修饰的类**无法被继承**

#### （二）final修饰方法

- `final`修饰的方法**无法被覆盖**

  > 强行覆盖会报错：<u>被覆盖的方法为`final`</u>

#### （三）final修饰变量

- *局部变量*

  - **只能赋值一次**

- *实例变量（不常用）*

  - 必须**显式赋值**，不能依靠<u>系统默认值</u>

    > 什么是**显式赋值**？
    >
    > 在**定义**实例变量时给<u>自定义默认值</u>
    >
    > 在**构造方法**中给实例变量赋值
    >
    > （这两种赋值本质上都是在<u>构造方法执行时</u>赋值的）
    >
    > （当然，没有`final`修饰的情况下，系统默认值也是在构造方法执行时赋值的）

    > 如果用`final`修饰了实例变量，系统不会去给赋默认值
    >
    > 如果没有显示赋值，直接**报错**：<u>变量 未在默认构造器中初始化</u>

- *静态变量（常用）*

  - ==final经常与static联合修饰成员变量==

    > `final`修饰的成员变量本来也不会变化了，不如直接声明为静态变量，节省内存
    >
    > 加一个`static`成为类级别的，存储在<u>方法区</u>，**<u>在类加载时初始化</u>**（不是在构造方法执行时初始化的）

    > `static`与`final`联合修饰的成员变量称为“**常量**”
    >
    > 常量一般都用`public`修饰

---

---

## 抽象类和接口

---

---

### 一、抽象类

---

#### （一）抽象类概述

- *引用数据类型*

- *来源*

  - 将具有共同特征的**类进一步抽象**，得到抽象类

- *特点*

  - “类”本身就是一种抽象的概念，所以“抽象类”<u>无法</u>进行所谓的“<u>实例化</u>”

    > `new`不了，但抽象类是有**构造方法**的，用来**初始化子类对象的父类型特征**

  - 抽象类就是要被继承的，<u>与`final`对立</u>

#### （二）应用

- :star:*抽象类中的构造方法*

  - 用来给子类`super(参数)`

  > 注：构造方法是不能被继承的，
  > 所以为了复用祖先的构造方法，抽象类也会提供构造方法（在其中调用`super`）

- :star:*抽象类中的抽象方法*

  - **抽象方法概念**：没有具体实现的方法

    > 抽象方法的方法体是空的

  - **抽象方法的特点**

    - 修饰符列表中<u>有`abstract`关键字</u>
    - 没有`{}`，<u>以`(参数);`结尾</u>

    > 抽象类中可以没有抽象方法
    >
    > 但<u>抽象方法只能存在于抽象类（或接口）中</u>

  - **抽象方法的应用**

    - ==一个非抽象类继承抽象类，必须将其抽象方法实现==

      > 不然会报错：`subclass`不是抽象的，并且未覆盖`superclass`中的抽象方法

    - 抽象类继承抽象类，当然就不必实现抽象方法了

- :star:*抽象类与多态联合*

  - 既然方法会被覆盖，不如直接搞成抽象的

    > **面向抽象编程**

---

### 二、接口

---

#### （一）接口概述

- `interface`，引用数据类型，<u>编译后也生成`.class`文件</u>

  ```java
  [修饰符列表] interface 接口名 {}
  ```

- 接口是完全抽象的，支持**多继承**

  > 抽象类是半抽象的，接口是特殊的抽象类

  > <u>一个接口</u>可以**继承多个接口**(`extends`)；<u>一个类</u>可以**实现多个接口**(`implement`)

- 接口内只包含**<u>常量</u>**和<u>**抽象方法**</u>

  > 接口中的常量可以省略`public static final`修饰符，默认就是常量
  >
  > 接口中的抽象方法可以省略`public abstract`修饰符，默认就是**公开的**、**抽象的**

  > 其实在`JDK8`之后，接口中也可以写**静态方法**，只要用`static`修饰即可，默认有`public`，但是，<u>接口中的静态方法无法被继承</u>，就是这样规定的（防止冲突）
  >
  > > 接口中的常量可以被继承，因而在使用时可能会出现冲突的问题
  > >
  > > ```java
  > > Reference to 'b' is ambiguous, both 'A.b' and 'B.b' match
  > > ```

- **调用者**与<u>接口</u>是***"has a"***的关系，**实现者**与<u>接口</u>是***"like a"***的关系

  > ***has a***：属性/实例变量的形式

  > 接口一般是对“行为”的抽象

#### （二）接口的应用

- :star:*非抽象类实现接口*

  - 非抽象类继承接口，必须**实现接口中的所有抽象方法**

    > ==被实现的方法必须用`public`修饰==

  - 一个类可以**同时实现多个接口**

    > 这种机制弥补了<u>`Java`类和类之间单继承</u>的缺陷

  > 如果是抽象类(`abstract`)实现(`implements`)接口，可以不去实现

- :star:*接口与多态联合*

  - **接口类型引用**指向**实现类型对象**，然后去调用相关方法

    > 面向接口编程

  - ==接口转型==：接口类型引用可以**强转成其他任何类型**的接口，能通过<u>编译</u>，但如果这两个接口没有**同时被指向的实现类型实现**，就会引发<u>`ClassCastException`</u>

    ```java
    public class Test {
        public static void main(String[] args) {
            A a = new A();
            B b = (B) a;//error: 不兼容的类型: A无法转换为B
            M m = new P();
            N n = (N)m;//可以通过编译，但会ClassCastException
            M m2 = new Q();
            N n2 = (N)m2;//没有任何问题
        }
    }
    class A {}
    class B {}
    interface M {}
    interface N {}
    class P implements M {}
    class Q implements M, N {}
    ```

    > 这种机制是为了方便实现类调用不同接口中的方法

    > 当然还是需要==`instanceof`运算符==去判断两个接口是否同时被该实现类型实现

- *同时继承和实现*

  - `extends`写在前，`implements`写在后


#### （三）接口在开发中的作用

- *面向接口编程*

  > 降低耦合度，提高可扩展性

- *接口离不开多态*

  > 这是因为**接口无法实例化**，想**初始化接口引用**必须借助多态
  >
  > （变量如果不初始化，是无法继续使用的）

- *解耦合*

  - 接口降低了调用者和实现者之间的耦合度

    > **调用者**和**实现者**之间依靠**接口**联系起来

    > 提高了项目开发的合作效率

---

---

## 包机制与访问控制权限

---

---

### 一、Package和import

---

#### （一）作用

- 方便程序/类的管理，按其功能进行分类

  > 不同的软件包有不同的功能

#### （二）用法

- ##### *语法*

  - `package`是**关键字**，后面加**包名**
  - 只能是源代码中**第一行**有效代码

- ##### *包名命名规范*

  - 公司域名倒序 + 项目名 + 模块名 + 功能名

    > 公司域名是具有全球唯一性的

- ##### *编译运行*

  - 编译命令`javac`当然只能编译相同目录下的`.java`文件

    > 此时<u>不太在乎</u>包机制的<u>位置问题</u>，语法过了就行

  - ==运行命令java必须找到对应的**完整类名**的类（带上**包名**的）==，相同目录不一定行

    > `package`关键字“改变”了`.java`文件中**对外的完整类名**，运行命令`java`后面又必须接**完整类名**（带**包名**）

    > 当然，同时也要求相关类存在于**包名所指的真实目录**下

  - **带包编译命令**：`javac -d . xxx.java`

    > `-d`：directory，代表“带目录编译”
    >
    > `.`：代表编译到当前目录下

    > 这样就会带包生成相关类

    > 当然运行时还是要用完整类名：`java 包名.文件内的类名`

- *`import`的使用*

  - 什么时候用

    - 要调用的类在**不同的包下**

    > <u>`lang`包会被自动`import`</u>，不需要程序员手动写出来

  - 怎么用

    - 语法：`import 包名`

      > 可以采用`*`的方式

    - `import`语句只能在**`package`语句之下**（如果有`package`和话），**`class`定义之上**

  > 如果不使用`import`关键字，调用**不同包**下的类，仍然要用**带包名的完整类名**
  >
  > （不然编译都过不去）

  > 注意：这里的“<u>不同包</u>”指的是`.class`文件在不同包下，与`.java`源文件无关
  
  > JDK1.5之后，import可以用于导入静态方法：（静态导入）
  >
  > `import 包名.类名.静态成员名/方法名`（可以用`*`替代）
  >
  > > 当然这一定程度上牺牲一可读性

---

### 二、访问控制权限

---

#### （一）访问控制权限修饰符总结

![](pics\访问控制权限.png)

> 横着和竖着都要看

> `public > protected > 默认 > private`
>
> 认为`protected > 默认`是因为，它们在本类、<u>同包</u>下都可以访问，但`protected`**扩展了子类**，使得<u>子类即使不在同包下也可以访问</u>

#### （二）访问控制权限修饰符的使用范围

- *属性*：4个都能用
- *方法*：4个都能用
- *类*：默认和`public`可以，`protected`和`private`不行

---

---

## Object类的常用方法

---

---

### 一、toString()方法

---

#### （一）源代码

```java
public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

> 默认实现的输出是：
>
> `类名@对象的内存地址(十六进制)`

#### （二）作用

- 将一个“***Java*对象**”转换为“**字符串**”的表示形式

  > <u>简洁、详实、易阅读</u>

- 建议所有类都**重写**些方法

---

### 二、equals()方法

---

#### （一）源代码

```java
public boolean equals(Object obj) {
        return (this == obj);
}
```

> 这样的判等方式不合适，十分需要**重写**

#### （二）作用

- 用来判断两个**对象**是否<u>相等</u>

  > 判断基本数据类型是否相等，直接使用`==`就可以
  >
  > 但对于引用数据类型，需要调用其`equals()`方法

#### （三）重写方式

> 建议遵循以下的步骤去逐个判断

1. 是否为**相同引用**

   > 先满足<u>自反性</u>

2. 是否***`null`***

3. 是否**同一类型**

   > 利用`instanceof`运算符

   > 因为02和03的判断是完全相同的分支，所以可以<u>写在一个`if语句`里</u>

4. **相关属性**是否相等

   > 这一步先要**向下转型**

> 举例：IDEA自动生成的重写
>
> ```java
> @Override
> public boolean equals(Object o) {
>     if (this == o) return true;
>     if (o == null || getClass() != o.getClass()) return false;
>     MyTime myTime = (MyTime) o;
>     return year == myTime.year && month == myTime.month && day == myTime.day;
> }
> ```

> $\rightarrow$重写要彻底：当类之间互相利用时，一定要将几个类的`equals()`方法都重写了

---

### 三、finalize()方法

---

#### （一）源代码

```java
@Deprecated(since="9")
protected void finalize() throws Throwable { }
```

#### （二）作用

- ***JVM中的GC***负责调用此方法，不需要程序员手动调用

  > 建议垃圾回收器启动的方法：`System.gc();`

- 当一个java对象<u>即将被GC回收</u>的时候，垃圾回收器会调用此方法

  > 一个时机：**<u>垃圾销毁时机</u>**
  >
  > 有点像C++中的<u>析构函数</u>

---

### 四、hashCode()方法

---

#### （一）源代码

```java
@HotSpotIntrinsicCandidate
public native int hashCode();
```

> 有`native`关键字，原生方法

#### （二）作用

- 将对象的**内存地址**经过<u>哈希算法</u>转换成一个数

---

### 五、clone()方法

---

#### （一）源代码

```java
@HotSpotIntrinsicCandidate
protected native Object clone() throws CloneNotSupportedException;
```

#### （二）作用

- 深克隆和浅克隆

---

---

## 匿名内部类

---

---

### 一、内部类

---

#### （一）什么是内部类

- 在类的内部又定义了一个新的变量。被称为内部类

#### （二）内部类的分类

- 静态内部类：类似于静态变量

- 实例内部类：类似于实例变量

  > 要先创建外部类才能创建内部类

- 局部内部类：类似于局部变量

  > 定义在方法体里面，在其他方法中不能使用

---

### 二、匿名内部类

---

#### （一）是什么

- **局部内部类**的一种，没有名字

#### （二）什么时候用

- 方法的一个<u>参数</u>是**接口类型引用**

  > 需要写一个接口实现类“才能”给方法传参数（在参数中写`new 实现类(...)`）

#### （三）怎么用

- 在对应方法的参数列表中，`new 接口名() {接口的实现}`

  > 举例：
  >
  > ```java
  > mm.mySum(new Compute() {
  >          @Override
  >          public int sum(int a, int b) {
  >              return a + b;
  >          }
  >      }, 200, 300);
  > ```

  > 把接口换成**抽象类**也行，然后去实现抽象类中的**抽象方法**

  > 本质上是创建了一个类的子类(extends or implements)，并重写某些方法，同时也可以添加某些方法，然后立刻创建该类的实例；

- 不建议使用的原因(但Lambda表达式还是会用)

  - 一个类没有名字，无法重复使用
  - 代码可读性太差

---

---

## 数组

---

---

### 一、概述

---

#### （一）基本点

- ==引用数据类型==，父类是`Object`，存放在堆内存中

- 一个“容器”，一组数据，**数据的集合**

  > 当然也可以存放引用类型数据（存地址）
  >
  > 此时就可以==**结合多态**==，在实际的堆内存中存放不同的子类型

- 一旦<u>创建</u>，**长度不可改变**

- ==元素类型统一==，==元素内存地址连续（特点）==，==第一个元素的内存地址==作为整个**数组的地址**

#### （二）优缺点

- 检索效率高

  > 因为元素**类型一致**，内存**地址连续**，所以可以用**数学计算**的方法直接定位下标

- 随机增删效率较低

  > 这是为了保证元素内存地址的**连续性**

  > 注意是**随机**增删效率低，而不是所有元素

- 无法存储超大数据量

  > 这还是因为元素内存地址的**连续性**，难以找到超大的连续内存空间

---

### 二、用法

---

#### （一）定义

- *语法*：`类型名[] 数组名`

#### （二）初始化

- ##### *静态初始化*

  - 语法：`类型名[] 数组名 = new 类型名[]{元素}`

    > 或者简写成`类型名[] 数组名 = {元素}`，这种简写方式只能这样用，因为 Java 是强类型语言

    > `new 类型名[]{元素}`也是一种**匿名数组**的写法（静态版）

- ##### *动态初始化*

  - 语法：`类型名[] 数组名 = new 类型名[元素个数]`

    > 每个元素最初是系统默认值

    > `new 类型名[元素个数]`也是一种**匿名数组**的写法（动态版）
  
  > 区分静态与动态：有没有`{}`，`[]`里写没写元素个数

#### （三）数组拷贝

- 数组拷贝的**效率很低**，所以创建数组时就要**估计**好数组中的**元素个数**

- 相关方法：`System.arraycopy(Object, int, Object, int, int)`

  ```java
  @HotSpotIntrinsicCandidate
  public static native void arraycopy(Object src,  int  srcPos,
                                      Object dest, int destPos,
                                      int length);
  ```

---

### 三、Arrays工具类

---

#### （一）快速排序

```java
public static void sort(int[] a) {
    DualPivotQuicksort.sort(a, 0, a.length - 1, null, 0, 0);
}
```

> 每个**基本数据类型**的数组都有相同的重载，这里只是用`int`举例

#### （二）二分查找

```java
public static int binarySearch(int[] a, int fromIndex, int toIndex,
                               int key) {
    rangeCheck(a.length, fromIndex, toIndex);
    return binarySearch0(a, fromIndex, toIndex, key);
}
```

```java
// Like public version, but without range checks.
private static int binarySearch0(int[] a, int fromIndex, int toIndex,
                                 int key) {
    int low = fromIndex;
    int high = toIndex - 1;
    while (low <= high) {
        int mid = (low + high) >>> 1;
        int midVal = a[mid];
        if (midVal < key)
            low = mid + 1;
        else if (midVal > key)
            high = mid - 1;
        else
            return mid; // key found
    }
    return -(low + 1);  // key not found.
}
```

#### （三）打印

```java
public static String toString(int[] a) {
    if (a == null)
        return "null";
    int iMax = a.length - 1;
    if (iMax == -1)
        return "[]";
    StringBuilder b = new StringBuilder();
    b.append('[');
    for (int i = 0; ; i++) {
        b.append(a[i]);
        if (i == iMax)
            return b.append(']').toString();
        b.append(", ");
    }
}
```

#### （四）拷贝（扩容）

```java
public static int[] copyOf(int[] original, int newLength) {
    int[] copy = new int[newLength];
    System.arraycopy(original, 0, copy, 0,
                     Math.min(original.length, newLength));
    return coy;
}
```

---

---

## 常用类

---

---

### 一、String相关类

---

#### （一）String类基本点

- *引用数组类型*

- *双引号括起来的字符串*

  - 都是一个`String对象`，是`final`的

    > 在使用时可以将其看成一种==**特殊的引用**==，保存的是==**方法区字符串常量池的地址**==

  - ==存储于**方法区**的**字符串常量池**中==

    > 为什么要有字符串常量池？
    >
    > 因为<u>字符串对象</u>**很常用**，为了**提高执行效率**
    >
    > （是<u>第一次</u>用了之后才会**在常量池中创建相关对象**）

- *`new String(参数)`得到的字符串*

  - 这样创建出来的<u>对象本身</u>还是存储在<u>堆区</u>中

    > ==`new`都是在堆内存中开辟空间==

    > ==**对象引用**==仍属于**局部变量**，还是存储在**栈**中，保存的是==**堆区地址**==

  - 但<u>堆区对象内部</u>其实保存了**字符串常量池**中的**地址**

#### （二）String类的常用构造方法

- `byte数组`系列

  ```java
  public String(byte[] bytes) {...}
  ```

  ```java
  public String(byte bytes[], int offset, int length) {...}
  ```

- `char数组`系列

  ```java
  public String(char value[]) {...}
  ```

  ```java
  public String(char value[], int offset, int count) {...}
  ```

> 当然最常用的创建`String对象`方式还是：`String 对象名 = "字符串内容"`

#### （三）String类的常用实例方法

- *针对<u>局部</u>的方法*（7个）

  - :star:`charAt(int)`：返回字符串指定位置的`char`值

    ```java
    public char charAt(int index) {...}
    ```

  - :star:`contains(CharSequence)`：是否包含子串`s`

    ```java
    public boolean contains(CharSequence s) {...}
    ```

    > 注意是**连续的子串**

  - :star:`starsWith(String)`和`endsWith(String)`：是否以子串为前缀/后缀

    ```java
    public boolean startsWith(String prefix) {...}
    ```

    ```java
    public boolean endsWith(String suffix) {...}
    ```

  - :star2:`indexOf(String)`和`lastIndexOf(String)`：**子字符串**第一次/最后一次出现处的索引（从0开始的下标）

    ```java
    public int indexOf(String str) {...}
    ```

    ```java
    public int lastIndexOf(String str) {...}
    ```

  - :star:`replace(CharSequence, CharSequence)`：用`replacement`替换字符串中所有`target`

    ```java
    public String replace(CharSequence target, CharSequence replacement) {...}
    ```

    > `replaceAll`类似，但第一个参数是正则表达式；
    >
    > `replaceFirst`是只替换第一个，第一个参数也是正则表达式；
    >
    > 注意，它们三个都不会影响原字符串

  - :star:`split(String)`：将字符串用`regex`进行拆分，返回`String数组`（拆分后不包括`regex`）

    ```java
    public String[] split(String regex, int limit) {...}
    ```

    > 相当于是从前向后扫描，<u>遇到`regex`就砍下去</u>，并把**前面的字符串**添加到数组中（如果前面是<u>空的就添加空串</u>），但**<u>不会在结尾添加空串</u>**

  - :star:`substring(int)和substring(int, int)`：从`beginIndex`到末尾/`endIndex-1`截取字符串

    ```java
    public String substring(int beginIndex) {...}
    ```

    ```java
    public String substring(int beginIndex, int endIndex) {...}
    ```

- *针对<u>整体</u>的方法*（8个）

  - `compareTo(String)`：按<u>字典顺序</u>比较两字符串，并返回大小结果

    ```java
    public int compareTo(String anotherString) {...}
    ```

  - :star:`equals(Object)`和`equalsIgnoreCase(String)`：字符串是否相等

    ```java
    public boolean equals(Object anObject) {...}
    ```

    ```java
    public boolean equalsIgnoreCase(String anotherString) {...}
    ```

    > 在JDK8中，`equals`方法底层调用了`compareTo`方法

  - :star:`getBytes()`：将字符串转换为`byte数组`

    ```java
    public byte[] getBytes() {...}
    ```

  - :star:`isEmpty()`：判断字符串是否为**空字符串**

    ```java
    public boolean isEmpty() {...}
    ```

  - :star:`isBlank()`：判断字符串是否为**空白字符串**

    ```java
    public boolean isBlank() {...}
    ```

  - :star:`toCharArray()`：将字符串转换成`char数组`

    ```java
    public char[] toCharArray() {...}
    ```

  - :star:`toLowerCase()`和`toUpperCase()`：转换成小/大写

    ```java
    public String toLowerCase() {...}
    ```

    ```java
    public String toUpperCase() {...}
    ```

  - :star:`trim()`：去除整个字符串前后的空白字符（不包括中间的）

    ```java
    public String trim() {...}
    ```

    > 不会影响原字符串

#### （四）String类的常用静态方法

- :star:`String.valueOf(int)`：将非字符串转换为字符串

  ```java
  public static String valueOf(int i) {...}
  ```

  > 对于一个引用数据类型来说，底层会调用其`toString()`方法

  > `System.out.println(...)`方法里就调用了`String.valueOf(...)`

#### （五）StringBuffer类与StringBuilder

- ##### *前言*

  - 如果频繁使用`+`拼接字符串，会大量占用方法区的内存，造成空间浪费
  - 用`StringBuffer类`或`StringBuilder`类就可以解决这类“**拼接**问题”

- ##### *缓冲原理*

  - 底层是`byte数组`，没有被`final修饰`

    > `String`对象底层的`byte数组`是有`final修饰的`

  - 指向`byte数组`的引用`value`可以指向继续其他数组，使得原数组内存被释放

- *初始化*

  - 构造方法

    ```java
    public StringBuffer() {
        super(16);
    }
    ```

    ```java
    public StringBuffer(int capacity) {
        super(capacity);
    }
    ```

    > 为了提高`StringBuffer`的存储效率，尽可能给定一个合适的**初始化容量**，从而减少<u>底层数组的扩容次数</u>

- *修改字符串*

  - :star:`append(...)`：追加字符串

    ```java
    public synchronized StringBuffer append(String str) {...}
    ```

  - `delete(int, int)`：删除下标从`start`到`end-1`的所有字符

    ```java
    @Override
    public StringBuilder delete(int start, int end) {...}
    ```

  - `deleteCharAt(int)`：删除指定下标位置处的字符

    ```java
    @Override
    public StringBuilder deleteCharAt(int index) {...}
    ```

  - `insert(int, char|String|char[])`：从指定下标开始插入

    ```java
    public StringBuilder insert(int offset, char c|String str|char[] str) {...}
    ```

  - `reverse()`：翻转

    ```java
    public StringBuilder reverse() {...}
    ```

  - `charAt(int)`：返回指定位置的字符

    ```java
    public char charAt(int index) {...}
    ```

  - `setCharAt(int, char)`：修改指定位置字符

    ```java
    public void setCharAt(int index, char ch) {...}
    ```

- ##### *两个类的区别*

  - `StringBuffer`中的方法有`synchronized`关键字修饰，表示是**线程安全**的

---

### 二、包装类

---

#### （一）概述

- ##### *为什么有*

  - 与引用数据类型相比，基本数据类型有时不通用
  - 将基本数据类型包装成**类**更加通用

  > 比如某个方法的<u>参数是`Object类型`</u>，那在`JDK5`之前<u>没有自动装箱</u>的时候，就不能传入基本数据类型

- ##### *都是什么*

  <img src="C:/Users/不怕晒的铃铛/Desktop/学习资料（真的）/JavaWeb路线/前后端分离基础项目/SGBlog/笔记/pics/8种包装类.png" style="zoom:80%;" />

  > 数字类型的父类都是*`java.lang.Number`*，另外两个类型的父类都是*`java.lang.Object`*

#### （二）Number类

> `java.lang.Number`作为六种数字类型的父类，有必要先研究一下

- *抽象类*

- *手动拆箱*

  - 利用`xxxValue`方法实现的

  ```java
  public abstract int intValue();
  public abstract long longValue();
  public abstract float floatValue();
  public abstract double doubleValue();
  public byte byteValue() {
      return (byte)intValue();
  }
  public short shortValue() {
      return (short)intValue();
  }
  ```

  > 这些方法用于==**将<u>引用</u>数据类型转换为<u>基本</u>数据类型（拆箱）**==
  >
  > （理解***"value"***这个单词，纯粹的值，基本数据类型）

> ==**装箱：将<u>基本</u>数据类型转换为<u>引用</u>数据类型**==
>
> 可以通过**包装类的构造方法**实现

#### ==（三）通过<u>Integer类</u>讲解包装类知识点==

##### 1、构造方法（手动装箱）

```java
@Deprecated(since="9")
public Integer(int value) {
    this.value = value;
}
```

```java
@Deprecated(since="9")
public Integer(String s) throws NumberFormatException {
    this.value = parseInt(s, 10);
}
```

> :star:只有*`int`*和*`String`*类型可以通过`Integer`的构造方法装箱，其他类型不行
>
> （当然，乱传`String`对象会出现==`NumberFormatException`==）

> 浮点的包装类可以传除了`boolean`以外的基本数据类型以及`String`类型

> `Character`只能传`char`，`String`都不行

> `Boolean`可以传`boolean`和`String`，对于`String`来说，若不是`"true"`，则都会转换成`false`（不会抛异常）

##### 2、:star:自动装箱与自动拆箱

- ==:star::star2::star:`JDK5`之后有了**自动装箱**和**自动拆箱**==

- *好处*

  - 不必再使用<u>包装类的构造方法</u>，和`Number类`中的`xxxValue`方法了

    > **赋值**时可以自动装箱与自动拆箱

  - <u>包装类</u>与基本数据类型进行<u>算术运算</u>的时候也会自动拆箱

    > **运算**时可以自动装箱与自动拆箱

  - `==`判等时：

    - 双方都是包装类：不能自动拆箱
    - <u>一个基本</u>类与<u>一个包装类</u>：可以自动拆箱

##### 3、包装类中的常量

```java
@Native public static final int   MIN_VALUE = 0x80000000;
```

```java
@Native public static final int   MAX_VALUE = 0x7fffffff;
```

> 其他<u>基本数据类型的最值</u>也以**<u>常量</u>**的形式存放在<u>包装类</u>中

##### 4、整数型常量池

- `Java`中为了提高效率，提前将`-128`到`127`之间的包装对象提前创建好，存放于“整数型常量池”中

- 使用这个区间内的数字时就不会再`new`了

  > 注意这个和自动装箱与自动拆箱没啥关系，**引用永远是引用**，**永远保存内存地址**，<u>必要时应认真区分基本类与包装类</u>

- `Integer`中其实是有一个静态内部类，其中还有个静态代码块

  ```java
  private static class IntegerCache {
      ...
      static {
         ... 
      }
      ...
  }
  ```

  > ***cache***：池、缓存
  >
  > 优点：直接用、**效率高**
  >
  > 缺点：耗费内存

##### 5、常用方法

- `intValue()`：手动拆箱

- :star:`Integer.parseInt(String)`：将`String`类型转换为`int`类型

  > 这里包装类只是作为一个<u>转化工具</u>

  > 这个方法**很常用**，因为经常要把从网页中得到的<u>数字字符串</u>，转化为真正的数字类型

  ```java
  public static int parseInt(String s) throws NumberFormatException {
      return parseInt(s,10);
  }
  ```

  > 要学会“照葫芦画瓢”：
  >
  > `Byte.parseByte(String)`
  >
  > `Short.parseShort(String)`
  >
  > `Long.parseLong(String)`
  >
  > `Float.parseFloat(String)`
  >
  > `Double.parseDouble(String)`

- `Integer.toBinaryString(int)`：十进制转换为其他进制

  ```java
  public static String toBinaryString(int i) {...}
  ```

  > 同理可以把`Binary`替换成`Octal`或`Hex`
  >
  > 输出数值不带“前导符”

  > 实际上`Object类`中的`toString()`方法内部就调用了`Integer.toHexString(int)`
  >
  > 传入的`int`参数是`hashCode()`
  >
  > （在`@`后面）

- `Integer.valueOf(int)`和`Integer.valueOf(String)`：向包装类`Integer`转换

  > 一个类静态的`valueOf`方法往往意味着<u>向这个类型作转化</u>

  > `String`、`Integer`、`int`之间的转化
  >
  > <img src="C:/Users/不怕晒的铃铛/Desktop/学习资料（真的）/JavaWeb路线/前后端分离基础项目/SGBlog/笔记/pics/Stirng与整数的转化.png" style="zoom:80%;" />

---

### 三、日期类

---

#### （一）概述

- `java.util.Date`

#### （二）构造方法

- 无参构造：`new Date()`
- 毫秒构造：可以去结合`System.currentTimeMillis()`

#### （三）Date转化成String

- ##### *意义*

  - 得到**具有一定格式的日期字符串**

  > 不要想着尝试去重写`Date`类的`toString`方法了

- :star:==*`java.text.SimpleDateFormat`*==

  - *设定格式*：利用构造方法创建对象（传入日期格式）

    ```java
    public SimpleDateFormat(String pattern) {...}
    ```

    > :star:格式：
    >
    > 年：`yyyy`
    >
    > 月：`MM`
    >
    > 日：`dd`
    >
    > 时：`HH`
    >
    > 分：`mm`
    >
    > 秒：`ss`
    >
    > 毫秒：`SSS`
    >
    > > 举例：`"yyyy-MM-dd HH:mm:ss SSS"`
    > >
    > > （在日期格式中，除了日期相关字母以外，其他的字符都会一模一样的输出）

  - *格式转化*：`format(Date)`方法

    ```java
    public final String format(Date date) {...}
    ```

#### （四）String转化成Date

> 还是要借助`SimpleDateFormat`这个类

- *设定格式*：（与上面一样）

- *转化*：`parse(String)`方法

  ```java
  public Date parse(String source) throws ParseException {...}
  ```

  > `String`对象中的内容要和设定的格式一致

#### （五）获取毫秒数

- *相关方法*：`System.currentTimeMillis()`

  > 从<u>1970年1月1日</u>0时0分0秒0毫秒到<u>当前系统时间</u>的总毫秒数
  >
  > （当然对于东八区来说是8时）

- *应用*

  - 统计一个方法的执行时长，进而可以去优化程序

    > 在调用方法前后记录毫秒数即可

  - 联想到`Date`类的有参构造方法，可以用于创建`Date`对象

    > 这样就可以获取任意时间的`Date`对象了

> 总结`System类`相关属性和方法：
>
> - 属性
>
>   - `System.out`：`System`类的一个静态变量
>
> - 方法
>
>   - `System.out.println(...)`
>
>     > 实际上`println`这个方法是`PrintStream`类的方法
>
>   - `System.gc()`：建议启动垃圾回收器
>
>   - `System.currentTimeMillis()`
>
>   - `System.exit(0)`：退出`JVM`

---

### 四、数字类

---

#### （一）数字格式化

- ##### `java.text.DecimalFormat`

- ##### *格式符*

  - 任意数字：`#`
  - 千分位：`,`
  - 小数点：`.`
  - 不够时补0：`0`

- ##### *步骤*

  - **设定格式**：构造方法
  - **格式转化**：`format(double)`方法

  > 举例：
  >
  > ```java
  > DecimalFormat df = new DecimalFormat("###,000.000");
  > System.out.println(df.format(1234.5));//1,243.500
  > System.out.println(df.format(123.4));//124.400
  > ```

#### （二）高精度

- ##### `java.math.BigDecimal`

- ##### *应用*

  - 常用于<u>财务软件</u>中

  > `double`已经不够用了

- ##### *使用方法*

  - **创建高精度数字对象**：构造方法

    > 最好使用<u>`String`参数</u>的构造方法，防止`double`类型的精度丢失

  - **运算**（调用方法）

    - 求和：`add(BigDecimal)`，返回值也是`BigDecimal类型`
    - 除法：`devide(BigDecimal)`

#### （三）随机数

- ##### `java.util.Random`

- 使用

  - **创建对象**：构造方法直接`new`

  - **产生随机数**：

    - `nextInt()`

    - `nextInt(int)`

      ```java
      public int nextInt(int bound) {...}
      ```

      > 产生$[0,~bound)$之间的随机数
      >
      > ***nextInt***含义是**下一个**`int`类型的数据是`bound`，下一个，**取不到**的

---

### 五、枚举类

---

#### （一）作用

- 一个行为的结果是**<u>离散型随机变量</u>**

  > 结果情况是有穷的，可知的

#### （二）用法

- ##### *定义枚举类*

  - 和<u>类的定义</u>相似

  - 也是一种**引用数据类型**，编译后也生成`.class文件`

  - 其中的每个值可以看成一个**常量**

    > 调用枚举值的方法也是`枚举类名.枚举值名`

    ```java
    package com.bjpowernode.javase.enum2;
    public enum Season {
      SPRING, SUMMER, AUTUMN, WINTER
    }
    ```

    > 注：***Enum types cannot be instantiated***

- ##### *使用枚举类*

  - 在高版本`JDK`中，枚举类可以与`switch`结合

    > 这种用法不常用

> 枚举类本质理解：
>
> ```java
> //public class Season extends Enum {
> public enum Season {
>     //提供枚举类的常量对象(必须写在类体的最上方)
>     //public static final SPRING = new Season("arguments");
>     //public static final SUMMER = new Season("arguments");
>     //public static final AUTUMN = new Season("arguments");
>     //public static final WINTER = new Season("arguments");
>     SPRING("arguments"),
>     SUMMER("arguments"),
>     AUTUMN("arguments"),
>     WINTER("arguments");
>     
>     //枚举对象的属性
>     private final String arguments;
>     
>     //私有化枚举类的构造方法
>     private Season(String arguments) {
>         this.arguments = arguments;
>     }
>     
>     //其他诉求
>     public String getArgs() {
>         return arguments;
>     }
>     
>     /*@override
>     public toString() {
>         return "常量对象的名字";
>     }*/
> }
> ```
>
> 


---

---

## 异常

---

---

### 一、异常概述

---

#### （一）什么是异常

- 程序执行过程中发生的**不正常情况**

- `Java`提供了<u>异常处理机制</u>，`JVM`会将异常信息**打印到控制台**上

  > 如果出现了异常却不吭声，这个语言的设计就很失败
  >
  > 但`Java`是很完善的

- *异常发生的本质*：==`new`了个**异常对象**并**打印到控制台**==

#### （二）异常的作用

- 根据异常完善程序，让程序更加**健壮**

  > 抓住异常后可以继续运行

#### （三）异常的存在形式

- ##### *类的形式*

  - 异常以**类的形式**存在，可以**创建对象**

  > 程序<u>发生不正常情况</u>时，`JVM`可以`new`**异常对象**，并**打印到控制台**上

  > 运用生活中的例子理解异常：
  >
  > 火灾——**异常类**
  >
  > A家发生了火灾——**异常对象**
  >
  > B家发生了火灾——**异常对象**

---

### 二、异常的继承结构

---

#### （一）继承结构

- 使用UML来描述

  > UML是**统一建模语言**，一种图标式语言，面向对象的编程语言都有UML
  >
  > UML图可以描述<u>类和类之间的关系</u>，<u>程序执行的流程</u>，<u>对象的状态</u>等

![](C:/Users/不怕晒的铃铛/Desktop/学习资料（真的）/JavaWeb路线/前后端分离基础项目/SGBlog/笔记/pics/异常继承结构.png)

#### （二）两种异常

> 注意：==异常都是发生在运行阶段的==
>
> 编译时无法真正`new`对象，当然不可以出异常

- ##### *编译时异常*

  - 也叫<u>受检/受控异常</u>
  - :star:必须在**编写程序**时处理好，不然<u>编译器</u>会报错
  - 发生概率较高

- ##### *运行时异常*

  - 也叫<u>未受检/非受控异常</u>

  - :star:写代码时可以处理也可以不处理，编译器不管

    > 如果所有异常都要在编写代码的时候处理，那太麻烦了

  - 发生概率较低

---

### 三、异常的处理方式

---

#### （一）throws

- ##### *抛给谁*

  - 抛给方法的调用者
  - 如果是`main`方法，就抛给`JVM`（是`JVM`调用的`main`方法），**`JVM`终止程序**

- ##### *怎么抛*

  - 在方法体前面使用`throws`关键字

    ```java
    修饰符列表 返回值 方法名(形式参数列表) throws 异常名 {...}
    ```

    > 如果出现异常，底层`new`对象之后会直接上抛

    > 可以抛**父类异常**，也可以**同时抛出多个异常**，用`,`隔开

  - **重写后的方法**不能比重写前的方法抛出<u>更宽泛的编译时异常</u>，可以更少/更窄

    > 宽泛：父类的、更多非同源的

    > `RuntimeException`及其子类不受限制，可以随便抛

- ##### *抛出后*

  - 一旦抛出异常，要么进入`catch`子句，要么<u>方法停止执行</u>
  - <u>调用者要处理异常</u>，不然编译器报错：`Unhandled Exception`

#### （二）try...catch

##### 1、基础

- *何时抓*

  - `main`中的异常一般都要抓

- *怎么抓*

  - 语法：

    ```java
    try {
        可能出现异常的代码块;
    } catch (异常名 e) {
        捕捉到异常后走的分支;
    }
    ```

    > `e`是一个**引用**，保存的是**异常对象的内存地址**

- *抓住后*

  - 方法跳入`catch`子句中继续执行
  - `try`中的语句就不能继续向下执行了

##### 2、深入

- *`catch`*

  - 可以`catch`**父类异常**

    > 本质是**多态**

  - 一个`try`子句可以接**多个`catch`子句**

    > 多个`catch`子句也是**有顺序的**：
    >
    > 从上至下、**从小到大**
    >
    > 不然执行不到的，没意义

    > 这样有利于**异常的<u>精确处理</u>**，便于程序的调度

  - 可以`catch`多个异常，用`|`分隔异常名

    > 在`JDK8`之后支持这种语法

- *`finally`子句*

  - **`try`的“遗言”**，必须和`try`一起出现，并且一定会执行

    > 语法上，只有`try`和`finally`，没有`catch`，也是可以的

    > :star:即使`try`中出现了异常，也会执行`fianlly`中的内容；

    > 就算`try`中有`return ...;`，也会先去执行`finally`子句，再回来`return`
    >
    > （除非直接`System.exit(0)`退出`JVM`）
    >
    > :star2:但程序执行结果必须遵循“**代码从上至下执行**”的原则：
    >
    > ```java
    > public static void main(String[] args) {
    > 	System.out.println(m());//结果是100，而不是101
    > }
    > public static int m() {
    >  int i = 100;
    >  try {
    >      return i;
    >  } finally {
    >      i++;
    >  }
    > }
    > /*
    > 反编译之后的效果：
    > public static int m() {
    > 	int i = 100;
    > 	int j = i;
    > 	i++;
    > 	return j;
    > }
    > */
    > ```

  - 通常在`finally`语句块中完成**资源的释放/关闭**

  > *`final finally finalize`*：
  >
  > `final`：关键字；表示最终的，不变的
  >
  > `finally` ：关键字；与`try`联合使用于<u>异常处理机制</u>中
  >
  > `finalize`：标识符；`Object`类中的一个实例方法`finalize()`，由`GC`负责调用

---

### 四、异常类与异常对象

---

#### （一）异常类的常用方法

- `getMessage()`：获取异常的**简单描述信息**

  ```java
  public String getMessage() {...}
  ```

  > 这个“<u>简单描述信息</u>”来自于`new`异常对象时**构造方法**中传入的信息
  >
  > ```java
  > Exception e = new Exception("Message...");
  > ```

- `printStackTrace()`：打印异常**堆栈追踪信息**

  ```java
  public void printStackTrace() {
      printStackTrace(System.err);
  }
  ```

  > 这是由后台的一个**异步线程**负责的，所以和其他语句的执行顺序可能不一致

  > 程序员调用方法打印异常堆栈追踪信息是**不会终止程序**的，还可以继续运行，==提高了程序的健壮性==

  > :star:异常堆栈追踪信息怎么看？
  >
  > **从上往下看**，<u>跳过SUN公司写的内容</u>（因为SUN公司写的不会出错），
  >
  > 越靠上的位置越接近<u>出错的根源</u>

  $\rightarrow$`printStackTrace()`方法的常见应用：

  ```java
  try {
      可能出现异常的代码块;
  } catch (异常名 e) {
      e.printStackTrace();//一般来说这一行是必写的，不然出了异常也不明显
      其他要继续执行的代码;
  }
  ```

#### （二）自定义异常类

- ##### *为什么要定义*

  - `JDK`内置的异常不够用
  - 实际开发中很多业务会出现`JDK`中没有的异常

- ##### *怎么自定义*

  1. 编写一个类继承`Exception`或`RuntimeException`
  2. 提供两个构造方法
     - 无参构造
     - 带`String s`参数的构造，并调用`super(s)`

  ```java
  public class MyException extends Exception {// or RuntimeException
      public MyException() {}
      public MyException(String s) {
          super(s);
      }
  }
  ```

- ##### ==*怎么应用自定义异常*==

  - 在程序执行到对应的**异常情况**时，`new`一个自定义异常并`throw`

    > 手动`new`对象并上抛：`throw new 自定义异常名(异常信息);`
    >
    > `throw`出来的异常一般会继续通过方法`throws`给调用者，**将异常信息传递出去**
    >
    > （自己`throw`自己`catch`没意义）

    > 自定义异常都是程序员自己`new`，非自定义异常SUN公司`new`好了
