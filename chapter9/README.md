# 第 9 章 状态管理：Vuex

**自行百度Vuex**    
**https://vuex.vuejs.org/zh/**

Store（仓库）、State（状态）、Mutations（变更）、Actions（动作）

例子：

```html
<template>
   <div id="app">
　　　<side></side>
　　　<content></content>
　 </div>
</template>
```

```javascript
   import Side from './Side.vue'
　 import Content from './Content.vue'
　 export default {
　　 components : {
　　　 Side,
　　　 Content
　　 }
　 }
</script>
```

创建Side子组件，components/Side.vue。

```html
<template>
　 <ul class="side list-unstyled">
　　 <li>增加</li>
　　 <li>删除</li>
　 </ul>
</template>
```

创建Content子组件，components/Content.vue。

```html
<template>
　 <div class="content">
　　 <div class="item" v-for="item in items">
　　　 {{ item.content }}
　　 </div>
　 </div>
</template>
```

## 创建并注入store

传统方式下，如果需要通过Side组件去添加Content组件中的item，只能依赖于根组件App来进行事件的监听和广播，这样既增加了耦合度，也使得Side组件和Content组件无法独立复用。
而在Vuex中，我们首先会增加Store这个概念，用于存储整个应用所需的信息，本例中将存储元素的列表。
首先，我们可以用npm先安装Vuex。

建立一个新文件vuex/store.js，代码如下：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

// 创建一个对象来保存应用启动时的初始状态    

const state = {
　 items: [],　// items为元素列表,    
　 name : "　// 应用名称    
}

// 用于更改状态的mutation函数    

const mutations = {
　 ….
};

export default new Vuex.Store({
　 state,
　 mutations
})
```

创建好store后，需要将其注入到我们的应用中，新建文件app.js，引入Vue,Vuex及根组件App.vue。

```javascript
import Vue from 'vue'
import store from './vuex/store'
import App from './components/App.vue'

new Vue({
　 store,
　 el: 'body',
　 components: { App }
})
```

## 创建action及组件调用方式

action能够通过分发（dispatch），调用对应的mutation函数，来触发对store的更新。
我们在相同目录下建立vuex/actions.js。

```javascript
export const addItem = ({ dispatch, store }, item) => {
　 dispatch('ADD_ITEM', item);
}

export const deleteItem = ({ dispatch, store}) => {
　 dispatch('DELETE_ITEM');
}
```

action函数也可以通过异步请求向后端获取数据，或读取store中其他的相关数据后再进行分发。例如：

```javascript
export const getDataFromServer = ({ dispatch, store}) => {
　 // 这里只是进行一个说明，你需要自己引入所需的异步请求方法
　 $.ajax({
　　 url : '/api/data',
　　 success : function(data) {
　　　 diapatch('FETCH_DATA', data);
　　 }
　 })
}
```

在Vuex中，组件不会直接修改store对象或者自身的状态，都是通过action的方法来进行分发。下面就来修改component/Side.vue文件，使之能调用action的方法。

```javascript
// 修改template，为增加、删除两个按键添加事件
<ul class="side list-unstyled">
　 <li @click="addItem({ content : Math.random()})">增加</li>
　 <li @click="deleteItem()">删除</li>
</ul>
<script>
import { addItem } from '../vuex/actions'

export default {
　 data() {
　　 return {
　　 }
　 },
　 vuex: {
　　 actions: {
　　 　 addItem,
　　 　 deleteItem
　　 }
　 }
}
</script>
```

由于之前已经注入了store，所以在子组件中，我们多了一个新的选项vuex。它可以包含一个actions属性，并将actions.js中定义的方法赋值进去，同时可以用于事件绑定。

## 创建mutation

action分发后就由mutation来对store进行更新。需要修改之前的vuex/store.js文件，补全在vuex/actions.js中对应的两种行为。

```javascript
const mutations = {
　 ADD_ITEM (state, item) {
　　 state.items.push(item);
　 },

　 DELETE_ITEM (state) {
　　 state.items.pop();
　 }
};
```

对比actions.js中的方法和mutations，可以看出action在调动dispatch的时候，需要准确地传入action的名称，并且需要和mutations对象中的属性保持一致。由于动作名称往往为常量，所以我们习惯用大写的形式来命名。在大型项目中，也会单独把动作名称集合抽象成一个模块，单独管理，例如抽象成vuex/mutation-type.js。

```javascript
export default {
　 ADD_ITEM : 'ADD_ITEM',
　 DELETE_ITEM : 'DELETE_ITEM'
　 …..
}
```

vuex/actions.js即可修改为：

```javascript
import { ADD_ITEM } from './mutation-type.js'

export const addItem = ({ dispatch, store }, item) => {
　 dispatch(ADD_ITEM, item);
}
```

vuex/store.js中的mutations可修改为：

```javascript
import { ADD_ITEM } from './mutation-type.js'

const mutations = {
　 [ADD_ITEM](state, item) {
　 …….
}
```

## 组件获取state

组件中的vuex选项除了actions属性外，还有一个getters属性，里面可以定义函数，接受的参数即为vuex/store.js中定义的state对象。

修改components/Content.vue如下：

```javascript
export default {
　 vuex: {
　　 getters: {
　　　 // 这里采用的ES6的写法，你可以替换成
　　　 // items : function(state) { return state.items }
　　　 items: state => state.items
　　 }
　 }
}
```

这样我们在这个组件实例中就获得了state中的items数组，在template中就可以直接使用\<li v-for=“item in items”\>\<\/li\>来遍历数据。

除了在组件中直接声明getters函数外，也可以将其抽象成一个模块。例如，新建一个vuex/getters.js：

```javascript
export function getItems(state) {
　 return state.items;
}
```

components/Content.vue即可修改成：

```javascript
import { getItems } from '../vuex/getters';
export default {
　 vuex: {
　　 getters: {
　　　 items: getItems
　　 }
　 }
}
```

getters的使用并不是强制规定，只是一种最佳实践。特别是对于大型应用来说，很多组件可以共用getters方法，这样state中的值如果发生了变化，也只需要修改一个getter方法即可，而不用修改所涉及的所有组件。

## 小结

总结一下整体的流程

+ 操作组件：单击组件按钮，调用组件中获取的action函数。

+ action dispatch：action函数不会直接对store数据进行修改，而是通过dispatch的方式通知到对应的mutation。

+ mutation：mutation函数则包含了对store数据的具体修改内容。

+ store/state：store是包含当前state的单一对象，数据更新后，自动通知到getter函数。

+ getter：getter函数从store获取组件所需的数据。

+ 组件展示：组件中使用getter函数，获取新的数据，进行展示。


# 严格模式

Vuex.store具有严格模式，即当Vuex State在mutation函数之外的情况下被修改时，即会抛出错误。我们可以在创建实例时传入strict:true参数，即可开启严格模式：

```javascript
const store = new Vuex.Store({
　 //….
　 strict : true
})
```

需要注意的是，不要在生产环境中开启严格模式。严格模式会对state树进行一个深入观察，会造成额外的性能损耗，所以可以将上述例子修改为：

```javascript
const store = new Vuex.Store({
　 //….
　 strict : process.env.NODE_ENV !== 'production'
})
```

# 中间件

Vuex store接受middlewars选项来加载中间件，例如：

```javascript
const store = new Vuex.Store({
　 // ….
　 middlewares : [myMiddleware]
})
```

myMiddleware是一个对象，可以包含设定好的钩子函数，例如：

```javascript
const myMiddleware = {
　 onInit(state) {
　　 // 在初始化的时候被调用，可以记录初始state
　　 console.log(state);
　 },
　 onMutation(mutation, state) {
　　 // 每个mutation之后都会调用
　　 // 每个mutation参数格式为{ type, payload}
　　 console.log(mutation, state);
　 }
}
```

## 快照

可以在中间件内设置获取state的快照，用来比较mutation执行前后的state。只需在设置中间件对象的时候新增snapshot选项及onMutation钩子函数。

```javascript
const mySnap = {
　 snapshot: true,
　 onMutation (mutation, nextState, prevState) {
　　 // nextState和prevState分别为mutation触发前和触发后对原state对象的深拷贝
　 }
}
```

同严格模式一样，快照模式也建议只在开发模式下使用，处理方式与严格模式类似：

```javascript
const store = new Vuex.store({
　 //….
　 middlewares : process.env.NODE_ENV !== 'production' ? [mySnap] : []
})
```

## logger

为了方便调试和观察数据变化，Vuex自带了一个logger中间件，使用方法如下：

```javascript
// 使用的vuex版本是0.82
import createLogger from 'vuex/logger';

const store = new Vuex.Store({
　 middlewares : [createLogger()]
})
```

在调用action后，我们可以在控制台看到logger中间件输出的内容，记录了mutation的type和调用时间，以及state的变化过程：

createLogger有以下几个选项可供配置。

1. collapsed：默认为true，用于是否自动展开输出的mutations。

2. transformer：类型为函数，接受state为参数，用于限定在控制台输出的部分state。由于在大型应用中state通常会比较复杂，如果都直接输出到控制台会显得比较杂乱，所以可以用transformer进行控制。

3. mutationTransformer：类型为函数，接受mutation为参数，返回值即为控制台中输出的mutation。默认为{ type : ‘’, payload : ‘’}，我们也可以通过设定其返回值，来对控制台的输出进行自定义。例如，我们可以设置返回值为return mutation.type，这样在控制台中仅会输出mutation.type的值，而不输出mutation.payload

createLogger使用选项的具体示例如下：

```javascript
import createLogger from 'vuex/logger';

const logger = createLogger({
　 collapsed: false,
　 transformer (state) {
　　 return state.items
　 },
　 mutationTransformer (mutation) {
　　 return mutation.type
　 }
})

const store = new Vuex.Store({
　 middlewares : [logger]
})
```

# 表单处理

在Vuex的模式下，组件中的表单处理会稍显不同，因为表单“天然”的作用就是直接修改组件内状态，这和Vuex的action→mutation→state的修改方式显然并不符合。特别是在严格模式下。我们将第9.2节中的components/content.vue修改为：

```html
<template>
　 <div class="content">
　　 <div class="item" v-for="item in items">
　　　 <input type="text" v-model="item.content" />
　　 </div>
　 </div>
</template>
```

在用户输入时，就相当于直接修改state状态。而由于这个修改并不是在mutation中执行的，此时vuex就会抛出一个警告：

为了避免这种情况，也为了能够更好地跟踪state状态，我们会把表单元素绑定state的值，并在change或者blur事件中监听action行为，不推荐使用input，这样每次输入都会触发action，对性能消耗较大。例如：

```javascript
<input :value="item.content" @change="updateContent($index, $event.target.value)" >
//　components/content.vue 的js修改为：
import { updateContent } from '../vuex/actions'

export default {
　 vuex: {
　　 getters: {
　　　 items: state => state.items
　　 },
　　 actions: {
　　　 updateContent
　　 }
　 }
}
// vuex/actions.js中增加updateContent方法
export const updateContent = ({ dispatch }, index, value) => {
　 dispatch('UPDATE_CONTENT', index, value)
}
// vuex/store.js中增加mutations的UPDATE_CONTENT属性：
const mutations = {
// …..
　 UPDATE_CONTENT(state, index, value) {
　　 // 需要注意的是，我们在本例中修改的是items数组对象中content的值
　　 // 如果直接写成 state.items[index].content = value, vue是无法监听到数值变化的
　　 // 也就无法更新视图上的content值，所以此处用$set方式更新数据
　　 state.items.$set(index, { content : value });
　 }
};
```

当然，如果你觉得没有必要跟踪item.content的值，也可以不将此内容放入Vuex中，完全当做组件的本地状态。一般和其他组件不产生影响的状态就可以这么处理。

此外，如果希望使用状态管理，又想继续使用v-model，则可以通过Vue.js的计算属性来实现：

```javascript
// 修改components/content.vue
<div class="content">
　 <input type="text" v-model="appName">
　 …..
</div>
import { updateContent, updateName } from '../vuex/actions'
export default {
　 vuex: {
  　 getters: {
  　　 // ….
  　　 name : state => state.name
  　 },
  　 actions: {
  　　 // ….
  　　 updateName
　   }
　 },
　 computed: {
  　 appName: {
　  　 get() {
　　  　 return this.name;
  　　 },
　  　 set(val) {
　　  　 this.updateName(val);
  　　 }
　   }
　 }
}
// 修改vuex/actions.js
export const updateName = ({ dispatch }, name) => {
　 dispatch('UPDATE_NAME', name);
}
// 修改vuex/store.js
const mutations = {
　 // …..
　 UPDATE_NAME(state, value) {
　　 state.name = value;
　 }
};
```

这样既能使用v-model，又对state状态进行了跟踪。如果觉得每次input事件都调用action会引起性能损耗的话，也可以使用v-model本身的lazy修饰符来降低调用频率。

