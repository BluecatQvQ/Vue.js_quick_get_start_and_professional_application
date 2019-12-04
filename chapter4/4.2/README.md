<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 10:42:27
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 14:06:15
 * @Description: test
 -->
# 4.2 双向过滤器

之前提及的过滤器都是在数据输出到视图之前，对数据进行转化显示，但不影响数据本身。Vue.js也提供了在改变视图中数据的值，写回data绑定属性中的过滤器，称为双向过滤器。例如：

```javascript
<input type="text" v-model="price | cents" >
// 该过滤器的作用是处理价钱的转化，一般数据库中保存的单位都为分，避免浮点运算
Vue.filter('cents', {
　read : function(value) {
　　return (value / 100).toFixed(2);
　},
　write : function(value) {
　　return value * 100;
　}
});

var vm = new Vue({
　el : '#app',
　data: {
　　price : 150
　}
});

```

从使用场景和功能来看，双向过滤器和第2章中提到的计算属性有点雷同。而Vue.js 2.0 中也取消了过滤器对v-model、v-on这些指令的支持，认为会导致更多复杂的情况，而且使用起来并不方便。**所以Vue.js 2.0中只允许开发者在\{\{\}\}标签中使用过滤器，像上述对写操作有转化要求的数据，建议使用计算属性这一特性来实现。**