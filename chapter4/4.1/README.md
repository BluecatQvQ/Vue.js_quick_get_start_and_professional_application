<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 10:42:27
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 13:50:09
 * @Description: test
 -->
# 4.1 过滤器注册

Vue.js提供了全局方法Vue.filter()注册一个自定义过滤器，接受过滤器ID和过滤器函数两个参数。例如：

```javascript

Vue.filter('date', function(value) {
　if(!value instanceof Date) return value;
　return value.toLocaleDateString();
})

```

这样注册之后，我们就可以在vm实例的模板中使用这个过滤器了。

```javascript
<div>
　{{ date | date }}
</div>
var vm = new Vue({
　el : '#app',
　data: {
　　date : new Date()
　}
});
```

除了初始值之外，过滤器也能接受任意数量的参数。例如：

```javascript
Vue.filter('date', function(value, format) {
　var o = {
　　"M+": value.getMonth() + 1, //月份 
　　"d+": value.getDate(), //日 
　　"h+": value.getHours(), //小时 
　　"m+": value.getMinutes(), //分 
　　"s+": value.getSeconds(), //秒 
　};

　if (/(y+)/.test(format)) 
　　format = format.replace(RegExp.$1, (value.getFullYear() + "").substr(4 - RegExp.$1.length));
　for (var k in o)
　　if (new RegExp("(" + k + ")").test(format)) 
　　 　format = format.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
　return format;
});

```

使用方式即为：
```javascript
<div>
　{{ date | date 'yyyy-MM-dd hh:mm:ss'}}　//->　2016-08-10 09:55:35 即可按格式输出当前时间
</div>
```
