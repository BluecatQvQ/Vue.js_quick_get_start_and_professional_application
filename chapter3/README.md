<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-03 16:34:55
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-03 16:35:39
 * @Description: test
 -->
# 第 3 章 指令

指令是Vue.js中一个重要的特性，主要提供了一种机制将数据的变化映射为DOM行为。那什么叫数据的变化映射为DOM行为？前文中阐述过Vue.js是通过数据驱动的，所以我们不会直接去修改DOM结构，不会出现类似于`$('ul').append('<li>one</li>')`这样的操作。当数据变化时，指令会依据设定好的操作对DOM进行修改，这样就可以只关注数据的变化，而不用去管理DOM的变化和状态，使得逻辑更加清晰，可维护性更好。
Vue.js本身就提供了大量的内置指令来进行对DOM的操作，我们也可以开发自定义指令