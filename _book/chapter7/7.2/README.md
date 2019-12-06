# 7.2 Vue-resource

**Vue2.0推荐使用axios，不推荐使用Vue-resource**

**本节以Vue-resource 1.0.2版本进行说明。**

## 引用方式

```javascript
import VueResource from 'vue-resource';
Vue.use(VueResource);
```

## 使用方式

安装好Vue-resource之后，在Vue组件中，我们就可以通过this.$http或者使用全局变量Vue.http发起异步请求，例如：

```javascript
var List = Vue.extend({
  route : {
   // vue-router中的data钩子函数，
   data : function(transition) {
     //运行这段代码需要在服务器环境中，即localhost下，直接访问文件运行这段代码会抛出异常
     this.$http
         .get('/api/list?pageNo=' + transition.to.params.page);
         .then(function(rep){
            // 成功回调函数
            transition.next({
              list : rep.data
            });
         }, function(rep) {
            // 失败回调函数
            transition.next({
              data : rep.data
            });
         });
     }
  },
  template: '<h1>This is the list page</h1>'
})
```

this.$http支持Promise模式，使用.then方法处理回调函数，接受成功/失败两个回调函数，一般会在回调函数中再调用transition.next()方法，给组件的data对象赋值，并执行组件的下一步骤。

## $http的api方法和选项参数

this.$http可以直接当做函数来调用，我们以下面这个例子来对其选项进行说明：

```javascript
this.$http({
  url : '/api/list',   // url访问路径
  method : '',     // HTTP请求方法，例如GET,POST,PUT,DELETE等
  body : {},      // request中的body数据，值可以为对象，String类型, 也可以是FormData对象
  params : {}，   // get方法时url上的参数，例如/api/list?page=1
  headers: {},     // 可以设置request的header属性
  timeout : 1500,  // 请求超时时长，单位为毫秒，如果设置为0的话则没有超时时长
  before : function(request) {},  //请求发出前调用的函数，可以在此对request进行修改
  progress: function(event) {},  // 上传图片、音频等文件时的进度，event对象会包含上传文件的总大小和已上传大小，通常可以用来作为进度条效果
  credentials : boolean,  // 默认情况下，跨域请求不提供凭据(cookie、HTTP认证及客户端SSL证明等)。该选项可以通过将XMLHttpRequest的withCredentials属性设置为true，即可以指定某个请求强制发送凭据。如果服务器接收带凭据的请求，会用Access-Control-Allow-Credentials: trueHTTP头部来响应
  emulateHTTP: boolean,  // 设置为true后，PUT/PATCH/DELETE请求将被修改成POST请求，并设置header属性X-HTTP-Method-Override。常用于服务端不支持REST写法时
  emulateJSON : boolean  // 设置为true后，会把request body以application/x-www-form-urlencoded的形式发送，相当于form表单提交。此时http中的header的content-type即为application/x-www-form-urlencoded。常用于服务器端未使用application/json编码时
```

此外，this.$http还可以直接调用api方法，相当于提供了一些快捷方式，例如：

```javascript
get(url, [options])
head(url, [options])
delete(url, [options])
jsonp(url, [options])
post(url, [body], [options])
put(url, [body], [options])
patch(url, [body], [options])
```

以上方法均可以采用this.$http.get(url, options)或Vue.http.get(url, options)这样类似的形式进行调用。

在发起异步请求后，我们可以采用this.$http.get(…).then()的方式处理返回值。.then()接受一个response的参数，具体的属性和方法如下。
url: response的原始url。
body：response的body数据，可以为Object，Blob，或者String类型。
headers：response的Headers对象。
ok: 布尔值，当HTTP状态码在200和299之间时为true。
status: response的HTTP状态码。
statusText: response的HTTP状态描述。
另外还包含以下三种api方法。
text(): Promise类型，把response body解析成字符串。
json(): Promise类型，把response body解析成json对象。
blob(): Promise类型，把response body解析成blob对象，即二进制文件，多用于图片、音视频等文件处理。

## 拦截器

拦截器主要作用于给请求添加全局功能，例如身份验证、错误处理等，在请求发送给服务器之前或服务器返回时对request/response进行拦截修改，完成业务逻辑后再传递给下一步骤。Vue-resource也提供了拦截器的具体实现方式，例如：

```javascript
Vue.http.interceptors.push(function(request, next) {
  //修改请求
  request.method = 'POST';
  // 继续进入下一个拦截器
  next();
});
```

也可以对返回的response进行处理：

```javascript
Vue.http.interceptors.push(function(request, next){
  request.method = 'POST';
  next(function(response) {
    // 修改response
    response.body = '...';
  });
});
```

或者直接拦截返回response，并不向后端发送请求：

```javascript
Vue.http.interceptors.push(function(request, next) {
  // body 可自己定义，request.respondWith会将其封装成response，并赋值到response.body上
  next(request.respondWith(body, {
    status: 403,
    statusText: 'Not Allowed
  }));
});
```

## $resource用法

Vue-resource提供了一种与RESTful API风格所匹配的写法，通过全局变量Vue.resource或者组件实例中的this.$resource对某个符合RESTful格式的url进行封装，使得开发者能够直接使用增删改查等基础操作，而不用自己再额外编写接口。

## 封装Service层

在编写SPA应用中，我们通常会把和后端做数据交互的方法封装成一个Service模块，供不同的组件进行使用。我们可以新建一个文件夹api，将Service模块集中起来，并按资源进行分类。
