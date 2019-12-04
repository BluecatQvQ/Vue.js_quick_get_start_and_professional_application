<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 10:17:59
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 10:18:44
 * @Description: test
 -->
# 3.4 指令在Vue.js 2.0中的变化

由于指令在Vue.js2.0中发生了比较大的变化，所以本节单独来说明下这些情况。总得来说，Vue.js 2.0中的指令功能更为单一，很多和组件重复的功能和作用都进行了删除，指令也更专注于本身作用域的操作，而尽量不去影响指令外的DOM元素及数据。