# 7.3 Vue-devtools

在开发时我们通常需要观察组件实例中的data属性的状态，方便进行调试。但一般组件实例并不会暴露在window对象上，我们无法直接访问到内部的data属性；若只通过debugger或console.log方法进行调试难免太过低效。所以Vue.js官方出了一款chrome插件Vue-devtools，它可以在chrome的开发者模式下直接查看当前页面的Vue实例的组件结构和内部属性，方便我们直接观测。

**自行百度**