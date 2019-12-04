<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 14:04:49
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 14:11:33
 * @Description: test
 -->
# 第 5 章 过渡

过渡系统是Vue.js为DOM动画效果提供的一个特性，它能在元素从DOM中插入或移除时触发你的CSS过渡（transition）和动画（animation），也就是说在DOM元素发生变化时为其添加特定的class类名，从而产生过渡效果。除了CSS过渡外，Vue.js的过渡系统也支持javascript的过渡，通过暴露过渡系统的钩子函数，我们可以在DOM变化的特定时机对其进行属性的操作，产生动画效果。