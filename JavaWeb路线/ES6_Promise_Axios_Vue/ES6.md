# ES6

---

---

---

## ECMAScript相关介绍

---

---

### 一、ECMA与ECMASciprt

---

#### (一) 什么是 ECMA

- ECMA（***European Computer Manufacturers Association***），**欧洲计算机制造商协会**

  > 94 年之后更名为 ECMA 国际

#### (二) 什么是 ECMAScript

- ECMAScript 是由 ECMA 国际通过 ECMA-262 标准化的**脚本程序设计语言**

---

### 二、为什么要学习ES6

---

- ES6 版本变动最多，具有**里程碑意义**
- ES6 加入许多<u>新的语法特性</u>，编程实现更简单、高效

---

---

## ES6新特性

---

---

### 一、变量相关

---

#### (一) let 关键字

- 语法：定义变量

  ```js
  let a;
  let b, c, d;
  let e = 100;
  let f = 521, g = 'iloveyou', h = [];
  ```

- 特性

  - 变量不能**重复声明**

    > 如果用 var 是可以重复的

  - **块级作用域**：只在`{}`包围的代码块里有效

    > let 定义的变量就属性块级作用域

    > ES5 中有三种作用域：全局，函数，eval

    ```html
    <script type="text/javascript">
    	let a = '250521';//这样可以在下面访问到
    	/*{let a = '250521';} 这么写下面就访问不到了*/
    </script>
    <!DOCTYPE html>
    <html>
    	<head>
    		<meta charset="utf-8">
    		<title></title>
    	</head>
    	<body>
    	</body>
    </html>
    <script type="text/javascript">
    	console.log(a);//250521
    </script>
    ```

    > 注：块级作用域不影响**作用域链**
    >
    > ```js
    > {
    >     let a = 250;
    >     function func() {
    >         console.log(a);//250
    >     }
    >     func();
    > }
    > ```
    >
    > > func 会向<u>上级作用域</u>中寻找

  - 不存在变量提升

    > 变量提升：代码执行之前先收集变量
    >
    > 举例：
    >
    > ```js
    > console.log(song);//undefined
    > var song = '520';
    > ```
    >
    > ```js
    > console.log(song);//Uncaught ReferenceError
    > let song = '520';
    > ```

- 案例：

  ```js
  for(var i = 0; i < items.lentgh; i++) {
      items[i].onclick = function() {
          itmes[i].style.background = 'pink';//下标i越界，i=length
          //用var声明的全局作用域，只有一份
      }
  }
  ```

  ```js
  for(let i = 0; i < items.lentgh; i++) {
      items[i].onclick = function() {
          itmes[i].style.background = 'pink';//向外层作用域寻找，找到let i=...;
      }
  }
  ```

> 综上，以后声明变量多用`let`

#### (二) const 关键字

- 语法

  ```js
  const MAX = 99;

- 特性

  - const 定义的是**常量**，定义时必须给**初始值**，且不能再**修改**

    > 但对于**数组**中的元素做修改，或对**对象**属性做修改，不算作对常量本身**地址**的修改

  - 属于**块级作用域**

#### (三) 变量解构赋值

- 解构赋值：ES6 允许按照一定模式从数组和对象中提取值，对变量进行赋值

  > 对于对象来说，这个**变量名**必须和**属性名**一致

- **数组解构**举例

  ```js
  const F4 = ['小损样', '刘能', '赵四', '宋小宝'];
  let [xiao, liu, zhao, song] = F4;
  console.log(xiao);
  console.log(liu);
  console.log(zhao);
  console.log(song);
  ```

- **对象解构**举例

  ```js
  const zhaobenshan = {
  	name: '赵本山',
  	age: 75,
  	xiaopin: function() {
  		console.log("play xiaopin");
  	},
  	dapin: function() {
  		console.log("lalal");
  	}
  }
  let {name, age, xiaopin} = zhaobenshan;
  // let{xiaopin} = zhaobenshan; 可以将方法单独解构出来
  ```

  > 注意：对象的解构变量必须与对象中的属性和方法**同名**
  
  > 除此之外，还可以
  >
  > - 在解构中**指定默认值**
  > - 将解构当成对象，作为函数返回值`return`

> 也建议用`const`修饰**数组**和**对象**

#### (四) 模板字符串

- 语法

  ```js
  let str = `I'm a string`;
  console.log(typeof str);//string
  ```

- 特性

  - 内容中可以直接出现换行符

    ```js
    let str = `<ul>
    			<li>沈腾<li>
    			<li>马丽<li>
    		   <ul>`;

  - :star:**字符串拼接**的新方式

    ```js
    let love = 'sd';
    let times = 521;
    let out = `i love ${love} ${times}`;
    console.log(out);//i love sd 521
    ```
    
    ```js
    let htmlStr = '';
    persons.forEach(p => htmlStr += `<li>${p.id}-${p.name}</li>`);
    let list = document.getElementById('list');
    list.innerHTML = htmlStr;
    ```
    
    > 可以用这种方式在前端拼接 HTML 代码

---

### 二、对象与函数相关

---

#### (一) 对象简化定义

- ES6 允许在**定义对象**时，在大括号里直接写入<u>之前定义好的</u>**变量名**和**函数名**，作为对象的**属性**和**方法**

  ```js
  let love = 'sd';
  let times = 521;
  let makelove = function() {
     console.log("FBI Open The Door !!!");
  }
  const lover = {
     love,
     times,
     makelove
  }
  ```

- 简化**方法**的定义

  ```js
  const lover = {
     love,
     times,
     makelove,
     improve() {
  	   console.log("improve");
     }
      /*
          improve: function() {
              console.log("improve");
          }
      */
  }
  ```

- **拼接**字符串作为属性名或方法名

  ```js
  const name = 'nam';
  const func = 'fu';
  const obj = {
      [name + 'e']:"myName",
      [func + 'nc']() {
          
      }
  }

#### (二) ==箭头函数==

- 语法

  ```js
  let 函数名 = (形式参数列表) => {
      函数体
  }
  ```

- 特性

  - 区别于普通函数，箭头函数中的 this 是**静态**的，始终指向函数**定义时所在作用域**下的上下文，==静态绑定==

    > **定义时**所在作用域，也就是`this`所在的**大括号的外面的那个大括号**的作用域，<u>不是调用时</u>了

    ```js
    function getName() {
       console.log(this.name);
    }
    let getName2 = () => {
       console.log(this.name);
    }
    window.name = 'shuaige';
    const star = {
       name: 'mingxing'
    }
    getName();//shuaige
    getName2();//shuaige
    getName.call();//shuaige
    getName2.call();//shuaige
    getName.call(star);//mingxing
    getName2.call(star);//shuaige
    ```

  - 不能作为**构造函数**去实例化对象

    ```js
    /* let Person = function(name) {
       this.name = name;
    } */
    let Person = (name) => {
       this.name = name;	
    }
    let me = new Person();//Uncaught TypeError: Person is not a constructor
    ```

  - 不能使用 ***arguments* 变量**

    ```js
    let fn = function() {
       console.log(arguments);//Uncaught ReferenceError: arguments is not defined
    }
    fn();
    ```

  - 箭头函数**简写**

    - 当<u>形参只有一个</u>的时候，可以**省略小括号**

    - 当<u>函数体只有一条语句</u>的时候，可以省略**大括号**，同时`return`和`;`也必须省略，这条语句的**执行结果**就是返回值

      ```js
      let fn = n => n * n;
      console.log(fn(9));//81
      ```

- 案例：普通函数手动保存`this`与<u>箭头函数**静态绑定**`this`</u>

  ```html
  <div id="ad" style="background: #58a;width: 300px;height: 300px;"></div>
  <script type="text/javascript">
  	let ad = document.getElementById("ad");
  	ad.addEventListener('click', function() {
          let _this = this;
          let func = function() {
              //谁调用谁是this，在window.setTimeout中this就是window
              _this.style.background = 'pink';
          };
          window.setTimeout(func, 1000);
  	})
  </script>
  ```

  ```html
  <div id="ad" style="background: #58a;width: 300px;height: 300px;"></div>
  <script type="text/javascript">
  	let ad = document.getElementById("ad");
  						    //如果把这个function也改了，this就会指向window
  	ad.addEventListener('click', function() {
  		let func = () => {
              //this与ad对象绑定
  			this.style.background = 'pink';
  		};
  		window.setTimeout(func, 1000);
  	})
  </script>
  ```

  ```js
  const result = arr.filter(item => item % 2 === 0);//过滤数组
  ```

> 总结：
>
> - 箭头函数适合与**实际调用者**无关的回调，比如**定时器**、**数组的方法回调**
>
> - 箭头函数不适合与**实际调用者**有关的回调，比如**事件回调**、**对象方法**
>
>   ```js
>   const obj = {
>   	name: 'shuaige',
>   	_this: this,
>   	getName: () => this.name
>   }
>   console.log(obj.getName());//<empty string>
>   console.log(obj._this);//Window
>   ```
>
>   ```js
>   const obj = {
>   	name: 'shuaige',
>   	_this: this,
>   	getName: function() {return this.name;}
>   }
>   console.log(obj.getName());//shuaige
>   console.log(obj._this);//Window
>   ```

#### (三) 函数初始值

- ES6 允许给**函数参数**赋**默认值**

  > 一般默认值要往后放，不然没意义

- 可以与对象的解构赋值结合

  > 举例：
  >
  > 首先，函数形参与对象解构结合
  >
  > ```js
  > function connect({host, username, password, port}) {
  > 	console.log(host),
  > 	console.log(username),
  > 	console.log(password),
  > 	console.log(port)
  > }
  > let connector = {
  > 	host: 'localhost',
  > 	username: 'root',
  > 	password: '123456',
  > 	port: '3306'
  > }
  > connect(connector);
  > ```
  >
  > 再在解构中给默认值
  >
  > ```js
  > function connect({host = '127.0.0.1', username, password, port}) {
  > 	console.log(host),
  > 	console.log(username),
  > 	console.log(password),
  > 	console.log(port)
  > }
  > let connector = {
  > 	//host: 'localhost',
  > 	username: 'root',
  > 	password: '123456',
  > 	port: '3306'
  > }
  > connect(connector);
  > ```

#### (四) rest 参数

- ES6 引入 rest 参数，可以用于获取函数的所有或部分**实参**，可以用来代替 arguments

- 语法

  ```js
  function func(a, b, ...args) {...}
  ```

  > rest 参数必须放在形式参数列表的**最后一个位置**
  >
  > 如果是唯一的，那就和 arguments 很像了，但 args 是**纯正的数组**

#### (五) 扩展运算符

- `...`扩展运算符能将**数组**转换为逗号分隔的**参数序列**

  > 这个**参数序列**，
  >
  > 可以作为**实参**传递给函数，
  >
  > 也可以作为**数组字面量**放在`[]`里面，与其他元素拼接也行，<u>浅拷贝</u>数组也行

  ```js
  const arr = [1, false, 'C9'];
  func(...arr);
  const arr1 = ["hit", "SZ", ...arr1, 985]
  const arr2 = [...arr1];
  ```

  > :star:重要应用：将**伪数组**转换为**真正的数组**
  >
  > ```js
  > const divs = document.querySelectorAll('div');
  > const divArr = [...divs];
  > ```
  >
  > > `getElementsBy...`系列的、`arguments`、... 都可以

- `...`扩展运算符还可以与解构赋值结合，将相当于将属性封装成数组赋值

  ```js
  let {a, ...arr} = obj;
  ```

- 使用扩展运算符需要相关类型**实现 iterator 接口**

---

### 三、扩展的数据类型和API

---

#### (一) Symbol 数据类型

- *概述*

  - ES6 引入**原始**数据类型 Symbol，表示独一无二的值

  - Symbol 的值是唯一的，用来<u>解决命名冲突的问题</u>

    > 但这个“唯一性”是在**内部**的，<u>对外不可见</u>

  - :star:最大的用途：定义对象的**私有变量**

- *创建 Symbol* 

  ```js
  let s = Symbol();//Symbol()返回的是symbol值,这个值很抽象,用s接收
  let s2 = Symbol('描述');//传入的字符串是描述一下这个抽象的symbol值
  let s3 = Symbol('描述');//虽然是同样的描述，但symbol值不同
  console.log(s2, typeof s2);//Symbol("尚硅谷") symbol
  console.log(s2 === s3);//false
  console.log(s2 == s3);//false
  let s4 = Symbol.for('描述');//通过'描述'得出专门为其而准备(for)的Symbol值
  let s5 = Symbol.for('描述');//专门为'描述'准备的，与s4相同
  console.log(s4, typeof s4);//Symbol("尚硅谷") symbol
  console.log(s4 == s5);//true
  console.log(s4 === s5);//true
  ```

  > `Symbol`是一个**函数对象**，可以直接作为**函数名**去调用，也可以后面加`.`访问属性

- *Symbol 应用：定义对象属性和方法*

  ```js
  let s1 = Symbol('property1');//这个s1是独一无二的symbol值
  let obj = {
      [s1]: 'value'//为了用到s1这个独一无二的值，只能[]
  }
  //obj[s1] = 'value'; //也行
  ```

  ```js
  let obj = {...};//里面有啥我不知道，但这里面的东西十分地珍贵，不能覆盖
  let s1 = Symbol('aProperty');
  let s2 = Symbol('aMethod');
  obj[s1] = ...;
  obj[s2] = () => {...};
  obj.s1;
  obj.s2();
  //obj[Symbol(aProperty)] = ...;      //这样的话这个symbol值就再也拿不到了
  //obj[Symbol(aMethod)] = () => {...};//这样的话这个symbol值就再也拿不到了
  ```

  > 注意，如果用 Symbol 定义了属性，一定要用`[]`的方式访问，`.`不出来（没这属性名）

- *Symbol 特性*

  - Symbol 值不能与其他数据进行运算，包括自身
  - Symbol 定义的<u>对象属性</u>不能使用 <u>for...in</u> 进行**枚举**，不能使用`Object.keys()`获取
  - 但可以使用如下两种方式来获取作为属性的 Symbol
    - `Object.getOwnPropertySymbols(obj)`
    - `Reflect.ownKeys(obj)`

- *Symbol 内置值*

  - `Symbol.hasInstance`

    - 类的内置方法（的 symbol 值）
    - 在使用 **instanceof 运算符**时会调用类的这个方法，同时传入参数代表<u>受检实例</u>

    ```js
    class Person {
        static [Symbol.hasInstance](instance) {
            //如果这里有return，instanceof的返回值也会受影响
        }
    }
    let o = {};
    console.log(o instanceof Person);
    ```

  - `Symbol.ConcatSpreadable`

    - 类的内置属性，一般用于数组
    - 表示数组是否可以展开

    ```js
    const arr1 = [1, 2, 3];
    const arr2 = [4, 5, 6];
    arr2[Symbol.ConcatSpreadable] = false;
    console.log(arr1.concat(arr2));//[1, 2, 3, [4, 5, 6]]
    ```

  - `Symbol.iterator`

    - 类的内置**方法**，用于创建**迭代器**

  > 还有好多内置值，可以查文档

#### (二) Set 类

- *概述*

  - 表示**无重复值**的**有序列表**

- *构造函数*

  ```js
  let set = new Set();
  let set = new Set(arr);
  ```

- *常用属性和实例方法*

  ```js
  set.size;
  set.add(anyTypeElem);//添加重复值时只会保留一个
  set.delete(anyTypeElem);
  set.has(anyTypeElem);//检验某个值是否在set中
  set.clear();
  let arr = [...set];//将set转换成数组（然后再遍历）
  for (let v of set) {...}//Set实现了iterator接口
  ```

- *应用案例*

  ```js
  let arr = [1,2,3,4,5,4,3,2,1];
  let arr2 = [4,5,6,5,6];
  //数组去重
  let result1 = [...new Set(arr)];
  //交集
  let result2 = [...new Set(arr)].filter(item => new Set(arr2).has(item));
  //并集
  let result3 = new Set(...arr, ...arr2);
  //差集
  let result4 = [...new Set(arr)].filter(item => 
                               !(new Set(arr2).has(item)) );

> ps：Set 中的对象引用无法释放
>
> ```js
> let set = new Set(), obj = {};
> set.add(obj);
> obj = null;//一般通过这种方式释放对象
> //现在set中仍然保存着原来obj的地址
> ```

> 补充：`WeakSet`
>
> - 不能传入非对象类型
> - 不可迭代，没有`forEach()`
> - 没有`size`属性

#### (三) Map 类

- *概述*

  - **键值对**的<u>有序列表</u>，键和值可以是任意类型

- *构造函数*

  ```js
  let map = new Map();
  let map = new Map([[key, value], [key, value], [key, value], ...]);
  ```

- *常用属性和方法*

  ```js
  map.size;
  map.set(key, value);
  map.get(key);
  map.has(key);
  map.delete(key);
  map.clear();
  for(let v of m) {...}//v的形式是[key, value]数组
  ```

---

### 四、扩展的方法和属性

---

#### (一) Object 新静态方法

- `Object.is(arg1, arg2)`：相当于`===`

  > 主要是解决了`NaN === NaN`返回`false`和`+0 === -0`返回`true`的问题

- :star:`Object.assign(targetObj, ...obj)`：将`obj`合并到`targetObj`中

  > 合并是指将属性都合并到一个对象中，也是**浅拷贝**

- `Object.setPrototypeOf(obj, proto)`和`Object.getPrototypeOf(obj)`：设置和获取原型对象

#### (二) Array 方法

- `Array.from(pseudoArr, callBackFunc)`：将伪数组转换成真正的数组（可以利用回调函数对每个元素处理后再返回）

  > ES5 中转换的方法：`var arr = [].slice.call(pseudoArr)`

- `Array.of(...elems)`：将一组任意数据类型值转换成数组

- `arr.find(elem => booleanExpressionWithElem)`：找出第一个符合条件的数组成员

  > 找全部可以用`filter`

- `arr.findIndex(elem => booleanExpressionWithElem)`：找出第一个符合条件的数组成员的索引

- `keys()`、`values()`、`entries()`：返回数组相关**迭代器**

  ```js
  for(let index of arr.keys()) {...}
  for(let elem of arr.values()) {...}
  for(let [index, elem] of arr.entries()) {...}
  ```

  > **迭代器**都可以使用`for...of`进行遍历

  > 当然也可以用`for...of`直接遍历数组（因为数组中有`Symbol.iterator`），可以直接返回元素**值**，而不是像`for...in`那样返回下标

- `includes(elem)`：判断数组是否包含元素

- `reduce((pre, current)=>{...}, initial)`：进行“**数据长度**”次的某种操作

  > 回调函数会执行数组长度次；
  >
  > initial是pre第一次的值，pre之后的值是回调函数的返回值；
  >
  > 最后一次的返回值就是reduce方法的返回值；
  >
  > current是每个数组元素

#### (三) 数值相关

- `Number.EPSILON`：JavaScript 的最小数字精度

  ```js
  function equal(a, b) {
      return Math.abs(a - b) < Number.EPSILON;
  }
  console.log(0.1 + 0.2 === 0.3);//false
  console.log(Object.is(0.1 + 0.2, 0.3));//false
  console.log(equal(0.1 + 0.2, 0.3));//true
  ```

- 进制

  ```js
  let b = 0b1010;
  let o = 0o777;
  let d = 100;
  let x = 0xff;
  console.log(b, o, d, x);//10 511 100 255
  ```

- `Number.isFinite()`和`Number.isNaN()`

  ```js
  console.log(Number.isFinite(100));//true
  console.log(Number.isFinite(100/0));//false
  console.log(Number.isFinite('中'));//false
  console.log(Number.isFinite(true));//false
  console.log(Number.isFinite(false));//false
  console.log(Number.isFinite(null));//false
  console.log(Number.isFinite(undefined));//false
  console.log(Number.isFinite(NaN));//false
  console.log(Number.isFinite(Infinity));//false
  ```

- `Number.parseInt(...)`和`Nubmer.parseFloat(...)`：将字符串转换为数字

  > 遇到不合法字符会截取并停止

- `Number.isInteger(...)`：**是否为整数**

  > 字符串类型的整数不算

- `Math.trunc(...)`：**抹除小数**部分

  > 允许字符串和 Boolean 类型；
  >
  > `Math.ceil(...)`是**向上取整**

- `Math.sign(...)`：判断一个数是**正**数、**负**数还是**零**

  > 允许字符串和 Boolean 类型


---

### 五、迭代器和生成器

---

#### (一) 迭代器

- *获取迭代器*

  ```js
  const it = items[Symbol.iterator]();
  ```

  > `Symbol.iterator`，是一个用于创建迭代器的方法，
  >
  > 重写这个方法时，调用后必须返回一个**对象**，这个对象必须有<u>`next`方法</u>，且`next`方法的返回值必须也是一个**对象**

- *使用迭代器*

  > 下面的使用是**迭代器的默认实现**

  ```js
  it.next();//return the 1st {value:..., done:false}
  it.next();//return the 2nd {value:..., done:false}
  it.next();//return the 3rd {value:..., done:false}
  it.next();//return the 4th {value:..., done:false}
  ...
  it.next();//return the last {value:..., done:false}
  it.next();//return {value:undefined, done:true}
  ```

  > 返回的是一个对象
  >
  > `done`为`true`时遍历结束，`value`变成`undefined`

- *应用：自定义对象迭代器遍历方式 / 重写迭代器*

  ```js
  const obj = {
      name: 'myName',
      arr: ['elem1', 'elem2', 'elem3'];
      [Symbol.iterator]() {
          let index = 0;
          //Result of Symbol.iterator must be an object
          return {
              next: () => {
                  if (index < this.arr.length) {
                      //Result of next must be an object
                      return {value: this.arr[index++], done: false};
                  } else {
                      return {value: undefined, done: true};
                  }
              }
          };
      }
  }
  for(let v of obj) {
      console.log(v);//elem1 elem2 elem3
  }
  ```

  > 注意，`for...of`直接拿到`next`方法返回的对象的`value`

#### (二) 生成器

- *概述*

  - 生成器其实就是一个特殊的函数，`generator`函数，可以通过`yield`关键字将函数挂起，为改变执行流提供了可能
  - `function * 函数名`，只能在函数内部使用`yield`

- *使用方法*

  - 调用迭代器`next`方法时才会执行

    ```js
    function * gen() {
        console.log('execute!');
    }
    let iterator = gen();//不会执行
    iterator.next()//execute!
    ```

  - `yield`会将函数体**分隔**，通过`next`方法<u>分批执行</u>，`next`方法返回封装了`yield`值和`done`的对象

    ```js
    function * gen() {
        console.log("block1");
        yield '我是分隔符';
        console.log("block2");    
        yield '我是分隔符';
        console.log("block3");
        yield '我是分隔符';
        console.log("block4");
    }
    let iterator = gen();//不会执行
    iterator.next();//block1
    iterator.next();//block2
    iterator.next();//block3
    iterator.next();//block4
    ```

    ```js
    function * gen() {
        console.log("block1");
        yield '我是分隔符';
        console.log("block2");    
        yield '我是分隔符';
        console.log("block3");
        yield '我是分隔符';
        console.log("block4");
    }
    for(let y of gen()) {//不会执行
        //block1 block2 block3 block4(block4之后不会再进入循环)
        //y就是yiled中的值
    }
    ```

  - 生成器函数本身和`next`方法都可以传入**参数**，`next`方法的参数会作为上一个`yield`语句 的返回结果

    ```js
    function * gen(arg) {
        console.log("block1");
        let y1 = yield '我是分隔符';
        console.log("block2");    
        yield '我是分隔符';
        console.log("block3");
        yield '我是分隔符';
        console.log("block4");
    }
    let iterator = gen('AAA');//let arg = 'AAA'
    iterator.next();//block1, return {value: '我是分隔符', done:false}
    iterator.next('BBB');//let y1 = 'BBB', block2, return {value: '我是分隔符', done:false}
    ```

- *应用举例*

  - 案例1：1s 后输出 111，再 2s 后输出 222，再 3s 后输出 333

    ```js
    /*回调地狱*/
    setTimeout(() => {
    	console.log('111');
    	setTimeout(() => {
    		console.log('222');
    		setTimeout(() => {
    			console.log('333');
    		}, 3000);
    	}, 2000);
    }, 1000);
    ```

    ```js
    /*抽取出三个任务*/
    function one() {
    	setTimeout(() => {
            console.log('111');
            //运行时会向上级作用域寻找iterator
            iterator.next();
        }, 1000);
    }
    function two() {
    	setTimeout(() => {
    		console.log('222');
    		iterator.next();
    	}, 2000);
    }
    function three() {
    	setTimeout(() => {
    		console.log('333');
    		iterator.next();
    	}, 3000);
    }
    function * gen() {
    	yield one();
    	yield two();
    	yield three();
    }
    let iterator = gen();
    iterator.next();
    ```
    
    ```js
    function getUsers() {
    	setTimeout(() => {
    		let data = '用户数据';
    		iterator.next(data);
    	}, 1000);
    }
    function getOrders() {
    	setTimeout(() => {
    		let data = '订单数据';
    		iterator.next(data);
    	}, 1000);
    }
    function getGoods() {
    	setTimeout(() => {
    		let data = '商品数据';
    		iterator.next(data);
    	}, 1000);
    }
    function * gen(msg) {
    	console.log(msg);
        //getUsers函数的“返回值”会被next中的参数替代
    	let usersData = yield getUsers();
    	console.log(usersData);
    	let ordersData = yield getOrders();
    	console.log(ordersData);
    	let goodsData = yield getGoods();
    	console.log(goodsData);
    }
    let iterator = gen('开始交易');
    iterator.next();
    ```
    
  

---

### 六、Promise

---

#### (一) Promise 是什么

- *抽象表达*

  - Promise 是一门新的**技术**（ES6 规范）

  - Promise 是 JS 中进行==**异步编程**==的新解决方案

    > 旧方案是单纯使用<u>回调函数</u>来处理

- *具体表达*

  - 从语法上来讲：Promise 是一个**构造函数**
  - 从功能上来讲：<u>Promise 对象</u>用来**封装一个异步操作**并可以获取其成功或失败的**结果值**

#### (二) 为什么要使用 Promise

- 支持==**链式调用**==，可以解决回调地狱问题

  > 回调地狱
  >
  > - 回调函数**嵌套调用**，外部回调函数异步执行的结果是嵌套的回调执行的条件
  > - 缺点：不便于异常处理；不便于阅读
  > - 解决方式：Promise 链式调用

- 指定回调函数的方式更加灵活

  > 旧方式：在启动异步任务前指定

  > Promise：
  >
  > 启动异步任务 $\Rightarrow$ 返回 Promise 对象 $\Rightarrow$ 给 Promise 对象绑定回调函数
  >
  > > 甚至可以在<u>异步任务执行结束之后</u>再指定<u>多个回调函数</u>来处理成功或失败的结果

#### (三) 基本使用方法

1. *创建 Promise 对象*

   ```js
   const p = new Promise((resolve, reject) => {//需要传入函数，带两个参数
   	...
   });
   ```

2. 进行**异步任务**，**判断**成功或失败，调用`resolve`或`reject`

   ```js
   const p = new Promise((resolve, reject) => {
       ...//异步任务
       //在成功的情况下调用resolve(value);
       //在失败的情况下调用reject(reason);
   });
   ```

   > 调用`resolve`或`reject`后会立即跳转，并修改<u>`Promise`对象</u>为**成功或失败的状态**

3. **处理**<u>异步任务</u>的**结果**

   ```js
   p.then(value => {
       ...
   }, reason => {
       ...
   });
   ```

   > 还有一个`catch`方法：
   >
   > ```js
   > p.catch(reason => {
   >     ...
   > });
   > ```
   >
   > > 相当于只捕获失败情况，一个语法糖

#### (四) :star:Promise 封装 Ajax 请求

```js
const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.onreadystatechange = () => {
        if (xhr.readyState === 4) {
            if (xhr.status >= 200 && < 300) {
                resolve(xhr.response);
            } else {
                reject(xhr.status);
            }
        }
    };
    xhr.open("GET", "URL");
    xhr.send();
});
p.then(value => {
    console.log("success!")
    console.log(value); 
}, reason => {
    console.warn("failure");
    console.warn(reason);
});
```

#### (五) :star:Promise.prototype.then方法

- 会返回一个<u>`Promise`对象</u>，对象状态由`then`中回调函数的**返回值**决定

  - 如果<u>回调函数</u>中返回的是一个<u>非`Promise`对象</u>，则`then`返回的`Promise`对象状态为成功

    > PromiseStatus: "resolved"
    >
    > PromiseValue: ...(返回的非Promise对象)

    > 注意，如果不写`return`，<u>默认返回结果是`undefined`</u>

  - 如果<u>回调函数</u>中返回的是一个<u>`Promise`对象</u>，则`then`返回的`Promise`对象状态与<u>回调函数返回的这个`Promise`对象</u>相同

    >PromiseStatus 和 PromiseValue 都保持一致

  - 如果<u>回调函数</u>中**抛**出错误，则`then`返回的`Promise`对象状态一定是`rejected`，对象值是抛出的错误信息

- 所以`then`方法是可以==**链式调用**==的，用于处理有序的**异步请求**

  ```js
  const p = new Promise((resolve, reject) => {
      //开启异步任务1
      //异步任务1成功或失败
  });
  p.then(value => {
      //异步任务1成功了
      return new Promise((resolve, reject) => {
          //开启异步任务2
          //异步任务2成功或失败
      });
  }, reason => {
      //异步任务1失败了
      return new Promise((resolve, reject) => {
          //开启异步任务2
          //异步任务2成功或失败
      });
  }).then(value => {
      //异步任务2成功了
      return new Promise((resolve, reject) => {
          //开启异步任务3
          //异步任务3成功或失败
      });
  }, reason => {
      //异步任务2失败了
      return new Promise((resolve, reject) => {
          //开启异步任务3
          //异步任务3成功或失败
      });
  }).then(value => {
      //异步任务3成功了
  }, reason => {
      //异步任务3失败了
  });
  ```

  > 流程：
  >
  > 开启异步任务$\Rightarrow$异步任务成功或失败$\Rightarrow$处理结果

---

### 七、class 类

---

> 从今往后写类就这么写！！！

#### (一) 概述

- 让对象原型的写法清晰，更有面向对象的感觉

  > 可以当成语法糖

#### (二) 创建类的语法

```js
class Phone {
    //这是构造函数的写法，必须是constructor
    constructor(brand, price) {//构造函数也可以不写，默认空实现
        this.brand = brand;
        this.price = price;
    }
    call() { //在class中就必须这样简写了
        console.log("打电话");
    }
}
```

#### (三) 静态成员

```js
/*ES5*/
function Phone() {
    
}
Phone.name = 'phone';
let nokia = new Phone();
console.log(Phone.name)//phone
console.log(nokia.name);//undefined
//只是给类扩展了静态成员，不在原型对象里，所以对象拿不到
```

```js
/*ES6*/
class Phone {
    static name = 'phone';
}
let nokia = new Phone();
console.log(Phone.name);//phone
console.log(nokia.name);//undefined
```

> **静态成员**属性<u>类</u>，不属于<u>原型对象</u>，也不属于<u>实例对象</u>

#### (四) 实现继承和重写

```js
/*ES5*/
function Phone(brand, price) {
    this.brand = brand;
    this.price = price;
}
Phone.prototype.call = function() {
    console.log('calling');
}
function SmartPhone(brand, price, color, size) {
    Phone.call(this, brand, price);//初始化父类型特征
    /*扩展子类特有的属性*/
    this.color = color;
    this.size = size;
}
/*同时修改原型和构造函数，实现继承（本质是重写原型对象）*/
SmartPhone.prototype = new Phone();
SmartPhone.prototype.constructor = SmartPhone;
/*扩展子类特有的方法*/
SmartPhone.prototype.photo = function() {
    console.log('photo');
}
```

```js
/*ES6*/
class Phone {
    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }
    call() {
        console.log('calling');
    }
}
class SmartPhone extends Phone {
    constructor(brand, price, color, size) {
        super(brand, price);
        this.color = color;
        this.size = size;
    }
    call() {//重写
        console.log('new calling');
    }
    photo() {
        console.log('phtot');
    }
}
```

> 重写后就不能直接调用父类同名方法了

#### (五) getter 和 setter

```js
class Phone {
    get price() {
        console.log('price is visited');
    }
    set price(value) {//A setter definition must have exactly one parameter
        console.log('price is set')
    }
}
let s = new Phone();
s.price;//price is visited
s.price = 'free';//price is set
```

---

### 八、模块化

---

#### (一) 概述

- 模块化是指将一个<u>大的程序文件</u>，拆分成许多<u>小的文件</u>，然后将小文件<u>组合</u>起来

  >这里的<u>小文件</u>就称为**模块**

- 模块化的好处

  - **封装作用域**、防止**命名冲突**
  - 代码**复用**
  - 提高**维护性**

#### (二) 语法

- *基础*

  - `export`：规定模块的对外接口
  - `import`：输入其他模块提供的功能

  ```js
  /* ./js/m1.js */
  export let school = 'hitsz';
  export function kungFu(somebody) {
      console.log(somebody + 'goHome');
  }
  ```

  ```js
  /* ./index1.html */
  <script type="module">
      import * as m1 from './js/m1.js';
      console.log(typeof m1);//object
  </script>
  ```

  > 注意这里要用`type="module"`，`text/javascript`不行

- *暴露语法*

  - **分别**暴露

  - **统一**暴露

    ```js
    /* ./js/m1.js */
    let school = 'hitsz';
    function kungFu(student) {
        console.log(' goHome');
    }
    export {school, kungFu};//对象的解构写法
    ```

  - **默认**暴露

    ```js
    export default {
        school: 'hitsz',
        kungFu() {
            console.log('goHome');
        }
    }
    ```

    > 这样访问时就要加一个`default.`
    >
    > 例如：`m1.default.school`

- *引入语法*

  - 通用的导入

    ```js
    import * as xxx from &#39;./xxx&#39;;
    ```

  - **解构赋值**形式，可用于分别暴露和统一暴露

    ```js
    import {school, kungFu} from './js/m1.js';
    console.log(school);//hitsz
    kungFu('we');//we goHome
    ```

    > 注意变量名必须一致，或者使用**别名**
    >
    > ```js
    > import {school, kungFu} from './js/m1.js';
    > import {school as c9, kungFu as pass} from './js/m1.js';
    > console.log(school);
    > kungFu('we');
    > console.log(c9);//hitsz
    > pass('nobody');//nobody goHome
    > ```

  - :star:接收`default`

    ```js
    import {default as c2} from "./js/m1.js";
    console.log(c2.school);//hitsz
    c2.kungFu('somebody');//somebody goHome
    ```

    ```js
    //简便写法
    import c2 from "./js/m1.js";
    ```
    
    > 所以，一般都会去写**默认暴露**

#### (三) 模块引入方式

- 直接在`script`中编写

  ```js
  <script type="module">
      //import...
  </script>
  ```

- **链入外部文件**

  ```js
  /* xxx.js */
  import ...;
  ```

  ```js
  /* xxx.html */
  <script src="./xxx.js" type="module">
  ```

> 解决兼容性问题的方法：见网课







































---

---

## ES7、ES8、ES9新特性

---

---















---

---

## ES10、ES11新特性