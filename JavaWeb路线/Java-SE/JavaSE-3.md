# JavaSE-3

---

---

---

## 反射

---

---

### 一、概述

---

#### （一）反射机制作用

- 可以通过反射机制读写**字节码文件**

  > 可以操作代码片段

- **让程序变得更加灵活**

#### （二）相关类

- `java.lang.Class`：代表**字节码文件**
- `java.lang.reflect.Method`：代表字节码文件中的**方法字节码**
- `java.lang.reflect.Constructor`：代表字节码文件中的**构造方法字节码**
- `java.lang.reflect.Field`：代表字节码文件中的**属性字节码**

#### （三）内存图

<img src="C:/Users/不怕晒的铃铛/Desktop/学习资料（真的）/JavaWeb路线/前后端分离基础项目/SGBlog/笔记/pics/反射内存图.png" style="zoom:80%;" />

---

### 二、Class类

---

#### （一）获取Class对象

- ##### *方式一*

  - :star:`Class.forName(String)`

    ```java
    public static Class<?> forName(String className)
                throws ClassNotFoundException {...}
    ```

    > 参数是一个类的**完整类名**（必须带有**包名**，`java.lang.`也不能省略）

    > 有**编译时异常**需要处理

    > `Class.forName(String)`会使类装载到`JVM`中，即会导致**类加载**，进而让类的**静态代码块执行**
    >
    > 所以，如果只是希望**一个类的静态代码块执行**，而不执行其他代码，可以利用这个方法

- ##### *方式二*

  - `Object`类中的实例方法`getClass()`

    ```java
    @HotSpotIntrinsicCandidate
    public final native Class<?> getClass();
    ```

    > `java`中任何一个对象都有这个方法，直接用**对象引用调用这个方法**，返回值就是这个类的<u>类型代表</u>

- ##### *方式三*

  - `class`属性

    > 举例：
    >
    > ```java
    > Class z = String.class; // z代表String类型
    > Class k = Date.class; // k代表Date类型
    > Class f = int.class; // f代表int类型
    > Class e = double.class; // e代表double类型
    > ```

#### （二）Class类常用方法

- ##### *针对类整体*

  - `Class.forName(String)`

  - `newInstance()`：通过`class`**对象**创建其代表**类的对象**

    ```java
    public T newInstance()
            throws InstantiationException, IllegalAccessException
        {...}
    ```

    > 底层实际上**调用**了代表**类的无参构造方法**
    >
    > > 所以要使用这个方法，必须保证**类的无参构造方法**是存在的，否则会出现`InstantiationException`

    > 与`new`创建对象相比，这个方法**更灵活**
    >
    > 可以结合配置文件与`properties`使用，读配置文件并创建对象
    >
    > 不必修改`Java`源代码，就可以创建不同的对象
    >
    > 符合***OCP*原则**

  - `getName()` / `getSimpleName()`：获取类的**完整类名** / **简类名**

    ```java
    public String getName() {...}
    public String getSimpleName() {...}
    ```

  - :star:`getSuperClass()`：获取**父类**代表

    ```java
    public native Class<? super T> getSuperclass();
    ```

  - :star:`getInterfaces()`：获取**接口**代表

    ```java
    public Class<?>[] getInterfaces() {...}
    ```

- ##### *针对类内部*

  - `getFields()`：获取类中所有**公开的属性**代表

    ```java
    public Field[] getFields() throws SecurityException {...}
    ```

    > 只能获取`public`修饰的属性代表

  - `getDeclaredFields()`：获取类中**所有的属性**代表

    ```java
    public Field[] getDeclaredFields() throws SecurityException {...}
    ```

  - `getModifires()`

    ```java
    public native int getModifiers();
    ```

    >还要结合`Modifier.toString(int)`方法
    >
    >```java
    >public static String toString(int mod) {...}
    >```
    >
    >这个方法会根据代表值，以`String`的形式返回**修饰符列表**，修饰符之间用**空格**隔开；如果是<u>默认修饰符</u>，会返回一个**空格**

  - :star:`getDeclaredField(String)`：获取类的**特定属性**

    ```java
    public Field getDeclaredField(String name)
            throws NoSuchFieldException, SecurityException {...}
    ```

  - `getDeclaredMethods()`：获取类的**所有方法**代表

    ```java
    public Method[] getDeclaredMethods() throws SecurityException {...}
    ```

  - `getDeclaredMethod(String, Class<?>...)`：获取类的**特定方法**代表

    ```java
    @CallerSensitive
    public Method getDeclaredMethod(String name, Class<?>... parameterTypes)
        throws NoSuchMethodException, SecurityException {...}
    ```

    > `Java`中方法是可以***overload***的，所以既需要**方法名**，又需要**形式参数列表**

  - `getDeclaredConstructors()`：获取类的**所有构造方法**代表

    ```java
    @CallerSensitive
    public Constructor<?>[] getDeclaredConstructors() throws SecurityException {...}
    ```

  - `getDeclaredConstructor(Class<?>...)`：获取类的**特定构造方法**代表

    ```java
    @CallerSensitive
    public Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)
        throws NoSuchMethodException, SecurityException
    {...}
    ```

#### （三）Class类的应用

> :star:==**通过反射机制实例化对象**==:star:

1. 读取**配置文件**中的类名

   - 可以使用比较基础的***IO*流**的方式读取，并**加载到*`properties`*对象**中

     > 详见：`IO`与`Properties`([链接](#九、IO与Properties))

   - 利用**当前线程**的**类加载器**对象，获取**类路径中的配置文件**的**输入流**，然后加载到`properties`对象中：

     `Thread.currentThread().getContextClassLoader().getResourceAsStream(String)`

     ```java
     @CallerSensitive
     public ClassLoader getContextClassLoader() {...}
     ```

     > 这是**线程对象的方法**，可以获取当前线程的**类加载器对象**；
     >
     > 想获取类加载器对象，也可以通过**字节码对象**的`getClassLoader()`方法

     ```java
     public InputStream getResourceAsStream(String name) {...}
     ```

     >这个方法的参数是**以类的根路径为起点**的路径，所以要求文件**必须在类路径里**

     > :star:补充：==类路径==
     >
     > - 凡是在`src`路径下的都是在类路径下的，**`src`是类的根路径**
     >
     > - 通过**类路径**获取**绝对路径**
     >
     >   `Thread.currentThread().getContextClassLoader().getResource(String).getPath()`
     >
     >   ```java
     >   public URL getResource(String name) {...}
     >   ```
     >
     >   > 这是**类加载器对象的方法**，当前线程的类加载器会默认从**类的根路径**下加载资源
     >
     >   > 这个方法的参数是**以类的根路径为起点**的路径，所以要求文件**必须在类路径里**
     >
     >   ```java
     >   public String getPath() {...}
     >   ```
     >
     >   > 这是**`URL`对象的方法**，返回**绝对路径**

   - 利用**资源绑定器**绑定**类路径下的配置文件**，获得**绑定器对象**，然后再获取**类名**

     `ResourceBundle.getBundle(String).getString(String)`

     ```java
     public static final ResourceBundle getBundle(String baseName) {...}
     ```

     > 这个方法的参数是**以类的根路径为起点**的路径，所以要求文件**必须在类路径里**，但是要注意，千万**不能写扩展名**，==不能加`.properties`==

     ```java
     public final String getString(String key) {...}
     ```

     > 这个参数`key`就可以理解成`Map`集合那样的`key`

2. 利用`Class.forName(String)`方法，获取`Class`对象

3. 利用`Class`对象的`newInstance()`方法，创建对象

> :star:补充：==类加载器==:star:
>
> - *什么是类加载器*
>
>   - 专门负责加载类的命令/工具
>
>     > ***ClassLoader***
>
> - *`JDK`中自带的3个类加载器*
>
>   - 启动类加载器：专门加载`rt.jar`中的类
>
>     > `jdk1.8.0_101\jre\lib\rt.jar`
>     >
>     > `rt.jar`中都是`JDK`最核心的类库
>
>   - 扩展类加载器：专门加载`ext\*.jar`中的类
>
>     >`jdk1.8.0_101\jre\lib\ext\*.jar`
>
>   - 应用类加载器：专门加载`classpath`中的类
>
>     >`classpath`是一个环境变量
>
> - *加载过程*
>
>   1. 首先通过**<u>启动类加载器</u>**加载
>   2. 如果通过**启动类加载器**加载不到，会继续通过**<u>扩展类加载器</u>**进行加载
>   3. 如果通过**扩展类加载器**还加载不到，那么最后会通过**<u>应用类加载器</u>**进行加载
>
> - *双亲委派机制*
>
>   - 优先从**启动类加载器**中加载，称为“父”
>
>     > 这是为了**保证类加载的安全**
>     >
>     > 否则<u>同名类</u>可能被“<u>植入</u>”
>
>   - “父”无法加载到，再从**扩展类加载器**中加载，称为“母”
>
>   - 双亲委派后如果都加载不到，才会考虑从**应用类加载器**中加载

----

### 三、Field类

---

#### （一）获取Field对象

- ##### *方式一*

  - `Class`对象的`getFields()`方法

    > 只包括`public`的，默认的都不行

  - `Class`对象的`getDeclaredFields()`方法

- ##### *方式二*

  - `Class`对象的`getField()`方法
  - `Class`对象的`getDeclaredField()`方法

#### （二）Field类常用方法

- `getName()`：获取**属性名**

  ```java
  public String getName() {...}
  ```

- `getType()`：获取**属性类型**代表

  ```java
  public Class<?> getType() {...}
  ```

- `getModifiers()`：获取属性前**修饰符列表**的代表值

  ```java
  public int getModifiers() {...}
  ```

  > 还要结合`Modifier.toString(int)`方法
  >
  > ```java
  > public static String toString(int mod) {...}
  > ```
  >
  > 这个方法会根据代表值，以`String`的形式返回**修饰符列表**，修饰符之间用**空格**隔开；如果是<u>默认修饰符</u>，会返回一个**空格**

- :star:`set(Object, Object)`：通过属性代表，**给对象的属性赋值**

  ```java
  public void set(Object obj, Object value)
      throws IllegalArgumentException, IllegalAccessException {...}
  ```

  > 三要素：对象、属性、值

- :star:`get(Object)`：通过属性代表，**获取对象的属性值**

  ```java
  public Object get(Object obj)
          throws IllegalArgumentException, IllegalAccessException {...}
  ```

- :star:`setAccessible(boolean)`：打破私有属性的封装

  ```java
  @Override
  @CallerSensitive
  public void setAccessible(boolean flag) {...}
  ```

  > 这也是反射机制的缺点

#### （三）Filed类的应用

- ##### *反编译类的属性*

  1. 通过`Class.forName(String)`方法获取类的`Class`对象
  2. 通过`Class`对象的`getDeclaredFields()`方法获取所有**属性**代表
  3. 借助`StringBuilder`进行**字符串拼接**，使用**循环**
  4. 通过循环中`Field`对象的`getModifiers()`方法和`Modifier.toString(int)`方法获取**修饰符列表**
  5. 通过循环中`Field`对象的`getType()`方法和`getSimpleName()`方法获取属性**类型**的简类名
  6. 通过循环中`Field`对象的`getName()`方法获取**属性名**

- :star:*读写`Java`对象的属性*:star:

  1. 通过`Class.forName(String)`方法获取类的`Class`对象

  2. 通过`Class`对象的`newInstance()`方法**创建对象**

  3. 通过`Class`对象的`getDeclaredField(String)`方法获取**特定属性**代表

     > 对于私有属性需要**打破封装**

  4. 通过`Field`对象的`get()`方法**获取属性值**

  5. 通过`Field`对象的`set(Object, Object)`方法**修改属性值**

---

### 四、Method类

---

> 可变长度参数：
>
> - 语法：`数据类型... 参数名`
>
> - 必须出现在形式参数列表的**最后一个位置**，且只能有一个
>
> - 参数的个数可以是**0个到可数无穷多个**
>
>   > 可以把这个参数看成<u>特殊的**数组**参数</u>，
>   >
>   > 也可以<u>传一个数组</u>，但没必要再这样做了

#### （一）获取Method对象

- 通过`Class`对象的`getDeclaredMethods()`方法
- 通过`Class`对象的`getDeclaredMethod()`方法

#### （二）Method类常用方法

- `getName()`：获取**方法名**

  ```java
  @Override
  public String getName() {...}
  ```

- `getReturnType()`：获取方法**返回值类型**代表

  ```java
  public Class<?> getReturnType() {...}
  ```

- `getModifires()`：获取方法**修饰符列表**的代表值

  ```java
  @Override
  public int getModifiers() {...}
  ```

  > 当然也要结合`Modefier.toString(int)`方法使用

- `getParameterTypes()`：获取方法的**形式参数列表**类型代表

  ```java
  @Override
  public Class<?>[] getParameterTypes() {...}
  ```

- :star:`invoke`：通过方法代表，**调用方法**

  ```java
  @CallerSensitive
  @ForceInline // to ensure Reflection.getCallerClass optimization
  @HotSpotIntrinsicCandidate
  public Object invoke(Object obj, Object... args)
      throws IllegalAccessException, IllegalArgumentException,
         InvocationTargetException
  {...}
  ```

  > 调用方法四要素：
  >
  > 对象、方法名、实际参数、返回值
  >
  > > 如果要调用的是静态方法，`obj`参数可以直接传`null`（传非`null`也没啥用）

#### （三）Method类应用

- *反编译类的方法签名*
  1. 通过`Class.forName(String)`方法获取类的`Class`对象
  2. 通过`Class`对象的`getDeclaredMethods()`方法获取所有**方法**代表
  3. 借助`StringBuilder`进行**字符串拼接**，使用**循环**
  4. 通过循环中`Method`对象的`getModifiers()`方法和`Modifier.toString(int)`方法获取方法的**修饰符列表**
  5. 通过循环中`Method`对象的`getReturnType()`方法和`getSimpleName()`方法获取**返回值类型**的简类名
  6. 通过循环中`Method`对象的`getName()`方法获取**方法名**
  7. 通过循环中`Method`对象的`getParameterTypes()`方法和`getSimpleName()`方法获取**形式参数列表**类型的简类名
- :star:*调用`Java`对象的方法*
  1. 通过`Class.forName(String)`方法获取类的`Class`对象
  2. 通过`Class`对象的`newInstance()`方法**创建对象**
  3. 通过`Class`对象的`getDeclaredMethod(String)`方法获取**特定方法**代表
  4. 通过`Method`对象的`invoke(Object, Object...)`方法**调用方法**

---

### 五、Constructor类

---

#### （一）获取Constructor对象

- 通过`Class`对象的`getDeclaredConstructors()`方法
- 通过`Class`对象的`getDeclaredConstructor()`方法

#### （二）Constructor类常用方法

- `getModifiers()`

  ```java
  public int getModifiers() {...}
  ```

- `getParameterTypes()`

  ```java
  public Class<?>[] getParameterTypes() {...}
  ```

- `newInstance(String)`：通过构造方法代表，**实例化对象**

  ```java
  public T newInstance(Object ... initargs)
      throws InstantiationException, IllegalAccessException,
             IllegalArgumentException, InvocationTargetException
  {...}
  ```

  > 这个方法相比于`Class`对象的`newInstance()`方法，可以传入参数，即可以**调用有参构造方法**

#### （三）Constructor类应用

- ##### *反编译类的构造方法*

- :star:*实例化`Java`对象*

  1. 通过`Class.forName(String)`方法获取类的`Class`对象
  2. 通过`Class`对象的`getDeclaredMethod(String)`方法获取**特定构造方法**代表
  3. 通过`Constructor`对象的`newInstance(Object...)`方法**实例化对象**

---

---

## 注解

---

---

### 一、概述

---

#### （一）注解是什么

- ***Annotation***
- **引用数据类型**，编译之后生成`xxx.class`文件

#### （二）注解怎么用

- ##### *自定义注解语法*

  ```java
  [修饰符列表] @interface 注解类型名{
      
  }
  ```

- ##### *使用方法*

  - 使用时语法：`@注解类型名`

  - 可以出现在**类**上、**属性**上、**方法**上、**变量**上等...

    > 注解还可以出现在**注解类型**上

---

### 二、JDK内置注解

---

#### （一）Override注解

- 源代码

  ```java
  @Target(ElementType.METHOD)
  @Retention(RetentionPolicy.SOURCE)
  public @interface Override {
  }
  ```

  > 表示一个方法声明打算**重写**超类中的另一个方法声明

  > 有`@Target`元注解，

- **只能注解方法**，如果这个方法不是<u>重写父类的方法</u>，编译器会**报错**

  > 所以可以用来<u>防止写错方法名</u>

- 这个注解是<u>给编译器参考的</u>，和运行阶段无关

#### （二）Deprecated注解

- 源代码

  ```java
  @Documented
  @Retention(RetentionPolicy.RUNTIME)
  @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
  public @interface Deprecated {
      String since() default "";
      boolean forRemoval() default false;
  }
  ```

  > 不鼓励程序员使用这样的元素，通常是因为它**很危险**或存在**更好的选择**

- 这个注解保存在`.class`文件中，可以被**反射**

- 使用已过时的元素会弹出`Warning`

  > `Warning`出现在提示信息`Messages`中

---

### 三、元注解

---

#### （一）概念

- 用来<u>标注注解类型</u>的注解，称为**元注解**

#### （二）常见元注解

- ##### *`@Target`*

  - 源代码

    ```java
    @Documented
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.ANNOTATION_TYPE)
    public @interface Target {
        /**
         * Returns an array of the kinds of elements an annotation type
         * can be applied to.
         * @return an array of the kinds of elements an annotation type
         * can be applied to
         */
        ElementType[] value();
    }
    ```

  - 用来标注<u>被标注的注解</u>可以出现在哪些位置上

  - 可以搭配的参数及含义：

    ```java
    @Target(ElementType.PARAMETER)
    @Target(ElementType.LOCAL_VARAIABLE)
    @Target(ElementType.FIELD)
    @Target(ElementType.METHOD)
    @Target(ElementType.Constructor)
    @Target(ElementType.Type)
    @Target(ElementType.ANNOTATION_TYPE)
    ```

- ##### *`@Retention`*

  - 源代码

    ```java
    @Documented
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.ANNOTATION_TYPE)
    public @interface Retention {
        /**
         * Returns the retention policy.
         * @return the retention policy
         */
        RetentionPolicy value();
    }
    ```

  - 用来标注<u>被标注的注解</u>最终保存在哪里

  - 可以搭配的参数及含义：

    ```java
    @Retention(RetentionPolicy.SOURCE)//只被保留在Java源文件中
    @Retention(RetentionPolicy.CLASS)//被保存在.class文件中
    @Retention(RetentionPolicy.RUNTIME)//被保存在.class文件中，并且可以被反射机制所读取
    ```


---

### 四、注解中的属性

---

#### （一）定义语法

```java
public @interface 注解名 {
    类型 属性名() default 默认值;
    ...
}
```

> 属性的类型可以是：`byte, short, int, long, float, double, boolean, char, String, Class, Enum`以及它们的数组类型

> `defalut 默认值`可以指定也可以不指定

#### （二）使用时的语法

- 如果一个注解中有**属性**，那么在使用该注解时，必须**给属性赋值**

  > 如果**指定了默认值**，那么在使用时，就**可以不手动赋值**了
  >
  > > 不用考虑顺序问题，因为赋值时必须写上`属性名`

- **赋值**语法

  ```java
  @注解名(属性名=属性值, 属性名=属性值, ...)
  ```

  > 注：
  >
  > - 如果**属性名**是`value`，并且**只有这一个属性**，可以省略`属性名`和`=`
  > - 如果数组属性值中<u>只有一个元素</u>，可以省略大括号

---

### 五、反射注解

---

#### （一）反射类上的注解

- 判断**类**上是否存在某个**注解**：`Class`对象的方法`isAnnotationPresent(Class<? extends Annotation)`

  ```java
  @Override
  public boolean isAnnotationPresent(Class<? extends Annotation> annotationClass) {...}
  ```

  > 直接传入`注解类名.class`即可

  > 只有`Retention`注解属性值指定为`RetentionPolicy.RUNTIME`的注解才可以被反射机制读取到

- 获取**类**的**注解对象**：`Class`对象的方法`getAnnotation(Class<A>)`

  ```java
  public <A extends Annotation> A getAnnotation(Class<A> annotationClass) {...}
  ```

- 获取**注解对象**的**属性值**：注解对象的方法`属性名()`

  > 定义注解中的属性时，语法格式很像<u>定义方法</u>，获取**属性值**时就可以像方法这样“**调用**”

#### （二）反射方法上的注解

1. 获取`Class`对象

2. 获取特定方法代表`Method`对象

3. 判断**方法**上是否存在某个**注解**：`Method`对象的`isAnnotationPresent(Class<? extends Annotation>)`

   ```java
   @Override
   public boolean isAnnotationPresent(Class<? extends Annotation> annotationClass) {...}
   ```

4. 获取**方法**的**注解对象**：`Method`对象的`getAnnotation(Class<T>)`方法

   ```java
   public <T extends Annotation> T getAnnotation(Class<T> annotationClass) {...}
   ```

5. 获取**注解对象**的**属性值**