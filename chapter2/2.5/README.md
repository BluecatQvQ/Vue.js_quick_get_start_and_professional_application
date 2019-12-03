<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-03 16:29:23
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-03 16:29:44
 * @Description: test
 -->
# 2.5 Vue.extend()

组件化开发也是Vue.js中非常重要的一个特性，我们可以将一个页面看成一个大的根组件，里面包含的元素就是不同的子组件，子组件也可以在不同的根组件里被调用。在上述例子中，可以看到在一个页面中通常会声明一个Vue的实例new Vue({})作为根组件，那么如何生成可被重复使用的子组件呢？Vue.js提供了Vue.extend(options)方法，创建基础Vue构造器的“子类”，参数options对象和直接声明Vue实例参数对象基本一致，

```javascript
    var Child = Vue.extend({
     template : '#child',
     // 不同的是，el和data选项需要通过函数返回值赋值，避免多个组件实例共用一个数据
     data : function() {
      return {
       ….
      }
     }
     ….
    })
    Vue.component('child', Child)　// 全局注册子组件
    <child ….></child>　// 子组件在其他组件内的调用方式    
```

更多组件的使用方法将会在第6章中进行详细的说明。