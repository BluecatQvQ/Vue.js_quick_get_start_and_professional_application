<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 10:42:27
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 14:08:33
 * @Description: test
 -->
# 4.4 过滤器在Vue.js 2.0中的变化

1. 取消了所有内置过滤器，即capitalize, uppercase, json等。
    作者建议尽量使用单独的插件来按需加入你所需要的过滤器。不过如果你觉得仍然想使用这些Vue.js 1.0中的内置过滤器，也不是什么难办的事。1.0源码filters/目录下的index.js和array-filter.js中就是所有内置过滤器的源码，你可以挑选你想用的手动加到2.0中。

2. 取消了对v-model和v-on的支持，过滤器只能使用在\{\{\}\}标签中。

3. 修改了过滤器参数的使用方式，采用函数的形式而不是空格来标记参数。例如：\{\{ date | date('yyyy-MM-dd') \}\}。