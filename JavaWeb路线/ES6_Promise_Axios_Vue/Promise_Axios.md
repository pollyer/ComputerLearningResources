# Promise_Axios

---

---

---

## Promise

---

---

### 一、概述

---

#### (一) Promise 是什么

- *抽象表达*

  - Promise 是一门新的**技术**（ES6 规范）

  - Promise 是 JS 中进行==**异步编程**==的新解决方案

    > 旧方案是单纯使用<u>回调函数</u>来处理

- *具体表达*

  - 从语法上来讲：Promise 是一个**构造函数**
  - 从功能上来讲：<u>Promise 对象</u>用来**封装一个异步操作**并可以获取其成功或失败的**结果值**

#### (二)为什么要使用 Promise

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

---

### 二、基本使用

---

#### (一) 基本使用方法

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
   >  ...
   > });
   > ```
   >
   > > 相当于只捕获失败情况，一个语法糖，可以拼接在`then`方法后面
   > >
   > > ```js
   > > p.then(value => {
   > >     
   > > }).catch(reason => {
   > >     
   > > });
   > > ```

#### (二) :star:Promise 封装 Ajax 请求

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

#### (三) :star:Promise.prototype.then方法

- 会返回一个<u>`Promise`对象</u>，对象状态由`then`中回调函数的**返回值**决定

  - 如果<u>回调函数</u>中返回的是一个<u>非`Promise`对象</u>，则`then`返回的`Promise`对象状态为成功

    > PromiseStatus: "resolved"
    >
    > PromiseValue: ...(返回的非Promise对象)

    > 注意，如果不写`return`，<u>默认返回结果是`undefined`</u>，会封装到`Promise`对象中

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

---

## Axios

---

---

### 一、概述

---

#### (一) 什么是 Axios

- 基于 Promise 的 HTTP **客户端**
- 可以在**浏览器**和 Node.js 环境中运行

#### (二) 特点

- 可以**在浏览器端发送 Ajax**
- 支持 **Promise 相关操作**
- 可以在请求前做**准备工作**，在响应后对结果**预处理**
- 可以对请求和响应数据做转换，可以<u>自动转换成 **JSON**</u>
- 可以取消请求
- 可以做保护，阻挡攻击

#### (三) 引入

- 在项目中，通过`npm`或者`yarn`

- 在学习中，使用 jsDelivr CDN

  ```js
  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  ```

  > 一般来说会切换成国内的资源
  >
  > ```js
  > <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.25.0/axios.min.js"></script>
  > ```

---

### 二、基本使用

---

#### (一) axios 函数

- *发送 GET 请求*

  ```js
  axios({
      method: 'GET',
      url: 'URL'
  }).then(value => {
      ...
  }, reason => {
      ...
  });
  ```

  > `axios({...})`函数会发送一个 Ajax 请求，参数是一个对象，需要指定相关属性，函数返回一个 Promise 对象

- *发送 POST 请求*

  ```js
  axios({
      method: 'POST',
      url: 'http://localhost:3000/posts',
      data: {
          ...: ...,
          ...: ...,
          ...
      }
  }).then(value => {
      ...
  }, reason => {
      ...
  });
  ```

> 还可以发送 PUT 请求、DELETE 请求，相当于“增删改查”全了

#### (二) axios.request 函数

- 与`axios({...})`的使用方法相同 

#### (三) HTTP 封装函数

- `axios.get(url, config)`

- `axios.post(url, data, config)`

  ```js
  axios.post('http://localhost:3000/comments',
      {
          "body": "so happy !",
          "postId": 2
      }).then(value => {
      console.log(value);
  }, reason => {
      console.warn(reason);
  });
  ```

---

### 三、重要对象介绍

---

#### (一) 响应结果的结构(成功)

> axios 会以 Object 的形式返回一个响应结果，就是上例中的 value（或者叫response）

- config
  - 配置对象（包括<u>请求内容</u>等很多信息）

- :star:data
  - **响应体**

  - axios 会自动将响应体以 <u>JSON</u> 格式的字符串**解析**，**封装为对象**

- header
  - 响应头

- request
  - 原生的 XMLHttpRequest 对象

- status
  - 响应状态码

- statusText
  - 响应状态字符串

> 如果是失败请求，返回的对象身上有`message`属性，表示失败信息

#### (二) config 对象相关属性

> 关于 axios 相关方法中的 config 参数，其中可以配置的属性

- :star:url

- :star:method

- :star:baseURL

  - 设定 URL 的基础结构
  - 相当于 url的起点，设置了之后，url 属性中就可以写**相对路径**了
  - axios 会<u>自动结合 url 与 baseURL</u>，生成最终请求的 URL

- transformRequest

  - 对请求的数据进行一定预处理

- transformResponse

  - 对响应的结果进行一定预处理

- :star:headers

  - 对**请求头**信息进行一定配置

  > 比如，默认的请求数据格式是：Content-Type: ‘application/json; charset=utf-8’，如果想改，就需要设置请求头中的`Content-Type`属性

- :star:params

  - 设置请求的 URL 的**参数**

  - 相当于在设置 <u>?name=value&name=value</u>

    > 这个参数要<u>传入一个封装对象</u>来设置

- paramsSerializer

  - 请求参数序列化对象
  - 可以用于改变 URL 参数格式

- :star:data

  - 用于设置**请求体**
  - 如果传入的参数是一个**对象**，则 axios 会将其转换成 **JSON 格式字符串**
  - 如果传入的参数是一个**字符串**，则直接发送

- :star:timeout

  - 超时取消，单位为毫秒

- withCredentials

  - 在跨域请求时对 Cookie 的携带进行设置

    > true 为可携带，false 为不可携带

- adapter

  - 对请求适配器进行设置

- auth

  - 对请求进行基础验证，比如可以设置用户名和密码

- responseType

  - 对响应体结果的格式进行设置
  - 默认为 `'json'`

- responseEncoding

  - 响应结果编码
  - 默认为 utf-8

- xsrfCookieName 和 xsrfHeaderName

  - 跨域请求相关配置

  - 这是一种<u>安全设置</u>，保证请求来自正常的客户端

    > 可以避免跨域攻击

- onUploadProgress 和 onDownLoadProgress

  - 在上传/下载时的回调函数

- maxContentLength 和 maxBodyLength

  - HTTP 响应体和请求体的的最大尺寸，默认值 2000

- validateStatus

  - 对响应结果的成功进行设置

    > 就是什么情况下认为这个请求响应是成功的

    ```js
    validateStatus: function (status) {
        return status >= 200 && status < 300; // default
    }
    ```

    > 这个默认情况就够用了

- maxRedirect

  - 最大重定向次数

- socketPath

  - 与 socket 相关的设置

- httpAgent 和 httpsAgent

  - 对客户端的信息进行一些设置

- proxy

  - 设置代理

    > 其实很有用，但我现在不会

- cancelToken

  - 取消 Ajax 请求

- decompress

  - 对响应结果解压，默认为 true

#### (三) 失败的响应结果

- `error.response`

---

### 四、进阶应用

---

#### (一) 默认配置

> 通过设置`axios.defaults`的属性，去进行默认配置，生效于所有需要`config`作为参数的 axios 函数；
>
> 下面列出几个常用的设置

- `axios.defaults.baseURL`
- `axios.defaults.method`
- `axios.defaults.params`
- `axios.defaults.timeout`

#### (二) 创建实例发送请求

- `axios.create(config)`：创建实例，这个实例的<u>功能与 axios 几乎相同</u>

  > 传入这个实例中的`config`就相当于**默认配置**了

#### (三) 拦截器

- *分类*

  - **请求**拦截器

    > 可以在<u>请求发送前</u>对请求的参数进行**检测**

  - **响应**拦截器

    >可以在<u>正式得到响应数据前</u>进行**预操作**

- *使用*

  ```js
  // Add a request interceptor
  axios.interceptors.request.use(function (config) {
      // Do something before request is sent
      return config;
  }, function (error) {
      // Do something with request error
      return Promise.reject(error);
  });
  
  // Add a response interceptor
  axios.interceptors.response.use(function (response) {
      // Any status code that lie within the range of 2xx cause this function to trigger
      // Do something with response data
      return response;
  }, function (error) {
      // Any status codes that falls outside the range of 2xx cause this function to trigger
      // Do something with response error
      return Promise.reject(error);
  });
  
  axios({
      //config 
  }).then(response => {
      //Do something with success
  }).catch(reason => {
      //Do something with failure
  });
  ```

  > 如果设置了多个拦截器，**请求**拦截器是<u>后设置的先执行</u>，**响应**拦截器是先<u>设置的先执行</u>

  > 注意
  >
  > - 请求拦截器的回调函数中有<u>`config`参数</u>，可以在请求发送前再次进行**配置**
  >
  > - 响应拦截器回调函数中有`response`参数，可以预先过滤一些信息
  >
  >   > 比如只返回`response.data`响应体

#### 	(四) 取消请求

```js
let cancel = null;
axios({
    ...
    cancelToken: new axios.CancelToken(c => cancel = c);
}).then(response => {
    ...
});
...
cancel();//当这个函数执行时，对应请求就会取消
```

> :star:应用：在<u>**上一次请求**还未结束</u>却又<u>重复发送**新请求**</u>时，**取消**上一次请求
>
> （只要判断`cancel`是否为`null`即可）
>
> ```js
> let cancel = null;
> ...
> if(cancel != null) {
>     cancel();
> }
> axios({
>     ...
>     cancelToken: new axios.CancelToken(c => cancel = c);
> }).then(response => {
>     cancel = null;
>     ...
> });
> ...
> cancel();
> ```

---

### 五、源码分析

---







​	













