<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 16:19:20
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 16:22:02
 * @Description: test
 -->
# 6.1 组件注册

Vue.js创建组件构造器的方式非常简单，在本书第2.5章的时候也提到过：

`var MyComponent = Vue.extend({ … });`

这样，我们就获得了一个组件构造器，但现在还无法直接使用这个组件，需要将组件注册到应用中。Vue.js提供了两种注册方式，分别是全局注册和局部注册。
