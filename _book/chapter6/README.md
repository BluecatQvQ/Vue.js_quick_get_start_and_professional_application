<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 16:19:20
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 16:20:38
 * @Description: test
 -->
# 第 6 章 组件

代码复用一直是软件开发中长期存在的一个问题，每个开发者都想再次使用之前写好的代码，又担心引入这段代码后对现有的程序产生影响。从jQuery开始，我们就开始通过插件的形式复用代码，到Requirejs开始将js文件模块化，按需加载。这两种方式都提供了比较方便的复用方式，但往往还需要自己手动加入所需的CSS文件和HTML模块。现在，Web Components的出现提供了一种新的思路，可以自定义tag标签，并拥有自身的模板、样式和交互。Angularjs的指令，Reactjs的组件化都在往这方面做尝试。同样，Vue.js也提供了自己的组件系统，支持自定义tag和原生HTML元素的扩展。