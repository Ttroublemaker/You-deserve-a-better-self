# AJAX

### 题目
- 书写一个简易的ajax
- 跨域的常用实现方式
  
### 知识点
- XMLHttpReguest
- 状态码
- 跨域：同源策略，跨域解决方案

### 1. XMLHttpReguest
```js
/** 1. 创建XMLHttpRequest对象 **/
var xhr = null;
xhr = new XMLHttpRequest()
/** 2. 连接服务器 **/
xhr.open('get', url, true)
/** 3. 发送请求 **/
xhr.send(null);
/** 4. 接受请求 **/
xhr.onreadystatechange = function () {
  if (xhr.readyState == 4) {
    if (xhr.status == 200) {
      success(xhr.responseText);
    } else {
      /** false **/
      fail && fail(xhr.status);
    }
  }
}
```

**xhr.readyState**
- 0－（未初始化）还没有调用send()方法
- 1－（载入）已调用send()方法，正在发送请求
- 2－（载入完成）send()方法执行完成，已经接收到全部响应内容
- 3－（交互）正在解析响应内容
- 4－（完成）响应内容解析完成，可以在客户端调用了
  
**xhr.status**

- **1xx（临时响应）表示临时响应并需要请求者继续执行操作的状态代码**
  - 100   （继续） 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。 
  - 101   （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。

- **2xx （成功）表示成功处理了请求的状态代码**
   - 200   （成功）  服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。
   - 201   （已创建）  请求成功并且服务器创建了新的资源。
   - 202   （已接受）  服务器已接受请求，但尚未处理。
   - 203   （非授权信息）  服务器已成功处理了请求，但返回的信息可能来自另一来源。
   - 204   （无内容）  服务器成功处理了请求，但没有返回任何内容。
   - 205   （重置内容） 服务器成功处理了请求，但没有返回任何内容。
   - 206   （部分内容）  服务器成功处理了部分 GET 请求。

- **3xx （重定向）表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向**
   - 301   （永久移动）  请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。
   - 302   （临时移动）  服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
   - 304   （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
   - 305   （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。
   - 307   （临时重定向）  服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。

- **4xx（请求错误）这些状态代码表示请求可能出错，妨碍了服务器的处理**
   - 400   （错误请求） 服务器不理解请求的语法。
   - 401   （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
   - 403   （禁止） 服务器拒绝请求。
   - 404   （未找到） 服务器找不到请求的网页。
   - 405   （方法禁用） 禁用请求中指定的方法。

- **5xx（服务器错误）这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错**
   - 500   （服务器内部错误）  服务器遇到错误，无法完成请求。
   - 502   （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。
   - 503   （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。


### 2. 跨域

#### 同源策略
- ajax请求时，浏览器要求当前网页和服务必须同源（安全）
- 同源：协议、域名、端口，三者必须一致
- 加载图片、css、js可无视同源策略
- 所有跨域，都必须通过服务端允许和配合
- 未经服务端允许就实现跨域，说明浏览器有漏洞，危险信号

#### 跨域方案
- JSONP
```js
  var script = document.createElement('script');
  script.type = 'text/javascript';

  // 传参并指定回调执行函数为onBack
  script.src = 'http://www.....:8080/login?user=admin&callback=onBack';
  document.head.appendChild(script);

  // 回调执行函数
  function onBack(res) {
    alert(JSON.stringify(res));
  }
```
- cors 服务器设置http header
![服务器设置http header](./imgs/js/跨域响应头设置.png)

- nginx代理跨域
- webpack-dev-server(仅适用于开发环境)

### 题目
- 书写一个简易的ajax
```js
function ajax (url) {
  const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url, true) // true表示异步请求
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status === 200) {
          resolve(JSON.parse(xhr.responseText))
        } else if (xhr.status === 404) {
          reject(new Error('404 not found'))
        }else if ( xhr.status === 500) {
          reject(new Error('500 server error'))
        }
      }
    }
    xhr.send(null)
  })
  return p
}
```
- 跨域的常用实现方式
   - JSONP
   - cors 服务器设置http header
   - nginx代理跨域
   - webpack-dev-server


### 3. fetch
### 4. axios