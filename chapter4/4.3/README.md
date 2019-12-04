<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 10:42:27
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 13:55:32
 * @Description: test
 -->
# 4.3 动态参数

过滤器除了能接受单引号（''）括起来的参数外，也支持接受在vm实例中绑定的数据，称之为动态参数。使用区别就在于不需要用单引号将参数括起来。例如：

```javascript
<input type="text" v-model="price" />
<span>{{ date | dynamic price }}</span>

Vue.filter('dynamic', function(date, price) {
　return date.toLocaleDateString() + ' : ' + price;
});
var vm = new Vue({
　el : '#app',
　data: {
　　date : new Date(),
　　price : 150
　}
});

```

过滤器中接受到的price参数即为vm.price。