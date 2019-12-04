<!--
 * @Author: zhanglingdi
 * @Date: 2019-12-04 10:42:27
 * @Email: 980583728@qq.com
 * @Company: Sinovatio
 * @version: v0.0.1
 * @LastEditors: zhanglingdi
 * @LastEditTime: 2019-12-04 13:45:11
 * @Description: test
 -->
# 第 4 章 过滤器

Vue.js允许在表达式后面添加可选的过滤器，以管道符表示，例如：

`{{ message | capitalize }}`

过滤器的本质是一个函数，接受管道符前面的值作为初始值，同时也能接受额外的参数，返回值为经过处理后的输出值。多个过滤器也可以进行串联。例如：

```javascript
{{ message | filterA 'arg1' 'arg2' }}
{{ message | filterA | filterB}}
```

