# JWT

## 1、概述

### 1.1 什么是JWT

JSON Web Token，
通过**数字签名**的方式，以***JSON*对象**为载体，
<u>在不同的服务终端之间安全地传输信息</u>

### 1.2 JWT有什么用

JWT最常见的场景就是==授权认证==，一旦用户登录，后续<u>每个请求都将包含JWT</u>，
系统在每次**处理用户请求**的之前，都要先进行<u>JWT安全校验</u>，通过之后再进行处理。

### 1.3 JWT的组成

JWT由<u>3部分</u>组成，用`.`进行拼接

3部分分别是：

- Header：Token类型 + 加密的算法名称

  ```json
  {
    'typ': 'JWT',
    'alg': 'HS256'
  }
  ```

  > 还会进行<u>base64编码</u>

  > 内容比较固定，可以被解密出来

- Payload(载荷)：存放自定义有效信息

  ```json
  {
    "sub": '1234567890',
    "name": 'john',
    "admin": true
  }
  ```

  > 分为标准注册声明、公共声明、私有声明；
  >
  > 也会进行<u>base64编码</u>

  > 可以被解密，不能存放敏感信息

- Signature：对*Header*、*Payload*(***base64*编码**后)和**密钥**(就是这个Signature)用**指定的算法**进行<u>加密</u>后得到的结果

  ```javascript
  var encodedString = 
      base64UrlEncode(header) + '.' + base64UrlEncode(payload);
  var signature = HMACSHA256(encodedString, 'secret');
  ```

  > **解密**时也是需要Signature的，利用设置签名时的key即可
  
  > 只要密钥不丢失，就可以认为整个JwtToken是安全的；
  >
  > JWT验证的主要部分就是Signature

## 2、使用

### 1.1 依赖与配置

依赖：

```xml
<dependency>
	<groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt</artifactId>
  <version>0.9.0</version>
</dependency>
```

> 对于JDK8以上的版本，还需要额外加3个依赖

### 1.2 加密

1. 创建`JwtBuilder`

   ```java
   JwtBuilder jwtBuilder = Jwts.builder();
   ```

2. 设置`JwtBuilder`的三部分、并拼接生成jwtToken

   ```java
   String jwtToken = jwtBuilder
     //header
     .setHeaderParam("typ", "JWT")
     .setHeaderParam("alg", "HS256")
     //payload
     .claim("username", "tom")
     .claim("role", "admin")
     .setSubject("admin-test")
     .setExpiration(new Date(1000 * 60 * 60 * 24 + 
                             System.currentTimeMillis()))
     .setId(UUID.randomUUID().toString())
     //signature
     .signWith(SignatureAlgorithm.HS256, "admin")
     //compact
     .compact();
   ```

   > 也可以使用<u>Map集合</u>的方式设置header和claim；
   >
   > 这样得到的jwtToken就是根据信息**加密**之后的token了；
   >
   > 注：如果设置`claim`的`key`为`"sub"`，则这个`claim`会自动成为`subject`

### 1.3 解密

1. 创建`JwtParser`

   ```java
   JwtParser jwtParser = Jwts.parser();
   ```

2. 提供签名key，解析jwtToken，获得数据集合`claims`

   ```java
   Claims claims = 
     jwtParser.setSigningKey("admin").parseClaimsJws(jwtToken).getBody();
   ```

3. 通过`claims`获取<u>map、主题、Id、有效期</u>等数据

   ```java
   Object username = claims.get("username");
   Object role = claims.get("role");
   String id = claims.getId();
   String subject = claims.getSubject();
   Date expiration = claims.getExpiration();
   ```

   

   

   

   













