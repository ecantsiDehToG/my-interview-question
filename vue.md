生命周期 组件传值
组件封装
虚拟列表的封装
双向绑定原理
diff算法
vuex的实现原理
vue-router的实现 hash模式（有锚点不发请求）和history模式

**1. 组件传值的方式**
最常见的情形是父子组件直接的传值，父组件传递给子组件是通过props属性绑定的方式，子组件传递给父组件是父组件预先在子组件中绑定事件，然后在子组件中通过$emit事件将值通过函数参数的形式传递，除此之外还有provide/inject的方式，parent和children也可以传值，然后$attrs和$listeners，另外就是eventBus，通过new一个空的vue实例对象作为事件总线（事件中心）$on监听事件并指定接收参数的回调函数，在组件中$emit来触发事件并将参数传递过去，或者状态管理工具vuex，所有组件都是可以获取到这些公共状态的。
补充:vuex 是 vue 的状态管理器，存储的数据是响应式的。但是并不会保存起来，刷新之后就回到了初始状态，具体做法应该在vuex里数据改变的时候把数据拷贝一份保存到localStorage里面，刷新之后，如果localStorage里有保存的数据，取出来再替换store里的state。看情况可以谈这个

**2. MVC, MVP, MVVM 模型是什么？**

​ 是框架模式，而不是设计模式

​ MVC（Model-View-Controller），模型-视图-控制器，达到业务逻辑、数据内容与界面显示分离的目的，如 BackBone，

​ MVP（Model-View-Presenter）,

​ 主要的区别：

​ MVC 中 View 会直接从 Model 中读取数据，不通过 Controller

​ MVP 中 View 和 Model 的通信全部通过 Presenter 进行，所有交互发生在 Presenter 内部

​ MVVM（Model-View-ViewModel）,ViewModel 负责把 Model 的数据同步到 View 显示出来，还负责把 View 的修改同步回 Model，如 react, vue 等

**3.key的作用**
在用遍历的方式生成子节点的时候，因为每个子节点都是类似的，绑定key就是使用唯一的值来标识这个节点，这样在更新节点进行diff的时候，就能知道每个节点是不一样的，否则从性能考虑会将节点进行简单的复用，这是vue在性能上做的一个优化，但在实际开发中，如果我们不设置一个唯一的key，或者我们简单地用数组遍历的index作为key，会产生一些意料之外的bug，比如我们设想这样一种业务场景，有一个遍历生成的表格，它是一个带校验的form表单，并且是动态增删它的行的，如果在写代码的时候，简单地给每一行绑定一个index，那你删除了这一行的时候，它的下一行的index变成跟它一样，它并不会真的如我们所想一样把当前这一行的dom节点删掉，然后后面的往前移，而是直接删最后一个，其余dom节点复用，只是在解析的时候把dom节点里面值换掉，但其实，带校验的form输入框是带校验的样式的，这样就产生了一些视觉上的bug。我们不希望这么做，所以应该给每个节点一个独一无二的key，这样不会简单地复用。

​**2. 你项目中 vue 是如何搭配其他技术使用的？**

jQuery，vuex，elementUI，iView，vue-router

  1. 主入口页面 index.html 中用 script 标签绝对路径引入，全局可用；

  2. webpack 中配置 plugin

  3. npm 安装，main.js 中引入，Vue.use()

**85. computed & watch 的区别**
从使用上来说，computed是侧重于函数的结果，当一个值依赖于其他的值的计算结果的时候，我们用计算属性；而watch侧重于函数的过程或者说函数的执行，如果某一个状态发生改变的时候，我们要做某些事，则使用watch。
从原理上来说，都是用watcher这个类来实现的，都可以监听页面数据的变化。computed 内部有lazy和dirty等标识符控制，基于它的依赖进行缓存，当依赖变化时才会重新计算，必须有返回值。
watch 是侦听一个特定的值，当值变化时来执行他的handler回调函数，里面封装了一些imidiate和deep等特性。watch常用来做例如分页组件中监听页码变化，获取对应数据，更换显示内容，还有监听\$route 变化，解决 created 生命周期钩子内函数只能执行一次导致的页面数据不刷新的问题。

**86. 生命周期钩子有哪些？你在项目中用过哪些？**

beforeCreate/created/beforeMount/mounted/beforeUpdate/updated/activated/deactivated/beforeDestroy/destroyed/errorCaptured
created发请求，如果涉及到操作dom要等到mounted发才行，activited/deactivated是配合keepalive来做缓存组件的时候经常要用到的，有缓存之后是不走created的，那就需要在activited中处理一些逻辑，还有destoryed也是会用到的，比如在组件中创建了定时器之类的，最好把这个timer的地址放到组件实例上，然后在销毁阶段将定时器销毁，我最近再掘金上看到还可以在其他的生命周期中通过hookEvent指定回调函数来写逻辑，用法是通过this.$on或者this.$once('hook:brforeDestroy',callback)的方式去监听所有的生命周期钩子函数
在父组件模板中中给子组件@hook:updated等="callback"还能监听到子组件内部的钩子

**87. 虚拟 dom 怎么实现的?**
虚拟dom就是用js对象来模拟真实dom，原因是访问和操作dom是很耗费性能的，我们去遍历一个dom对象的话可以看到有几百个属性，两三百个吧，而用js对象模拟真实dom的话会轻很多，与宿主浏览器无关，增删改查执行速度很快，这样由原本的操作dom变成操作v-dom，
vue 中，用 js 对象表示一个虚拟 dom 树，用这个树构建一个真实 dom 树，插入文档，每次需要更新视图时，会重新构建一个虚拟 dom 树，并将新树与老树对比，记录差异，把差异应用到第一步构建的真实 DOM 树上，视图就更新了。

**88. Vue.js 定义组件模板的除了单文件还有什么方法吗？**

\1. 字符串和模板字符串

Vue.component('my-checkbox', {

template: '<div></div>',

});

\2. X-template
```js

Vue.component('my-checkbox', {

template: '#idName',

});
```

\3. 内联模板

html 中给标签添加 inline-template 属性来告诉 vue 标签中的内容是模板：<my-check inline-template></my-check>

\4. 渲染函数
```js
  export default{
    data(){
      return {}
    },
    render(h){
      return h("div",{className:'box'},"hello world")
    }
  }
  // 这个h函数就是createElement函数
```

**89. render 函数使用过吗？怎么用的？**

在使用 elementui 的表格组件的时候使用过，表头自定义，使用 render 函数。跟react写jsx语法其实是差不多的
render函数接收h函数作为参数，返回h函数的调用结果，h函数调用接收三个参数标签名，属性对象，内容，

**90. vue-router 路由组件传参，接收方式？**

声明式导航<router-link :to="/"></router-link>

编程式导航 this.\$router.push({ path: 'register', query: { plan: 'private' }}) //相当于register?plan=private

**91. 为什么要用 vuex?**
公共数据管理，单项数据流，不是突然出现的， mvc 框架=>facebook 的 flux 框架=>react redux 等一系列发展

当多个状态（数据）分散在不同组件的各个角落，有大量状态（数据）需要相互间传递，如果都放在后台，统一去后台获取，则网络开销比较大，多层嵌套组件间就需要 vuex 这样的解决方案，公共数据托管在 state 里，不同组件都可以使用，vuex 有一个轻量型替代方案，bus.js。

常用场景：SPA 中音乐播放，购物车数据，登录状态，配合 localstorage 使用

**93. 数据双向绑定的原理？（只要能说把这个说清楚基本没问题）**

我是这么理解的，拿vue来说，他是接收到用户自己定义的options对象后，去递归给每个属性用Object.defineProperty去代理这个对象，重写get和set方法，在get中收集依赖，在set中驱动视图更新。它是结合发布订阅模式来实现的，可以这么理解，就是Object.defineProperty是实现数据代理的核心API，而发布订阅模式是组织代码的一种方式，它有个observer类，这个类就是我们实现发布订阅模式的构造方法，其中dep就是存放watcher实例的watcher池，然后它的订阅，就是在收集依赖的时候也就是get方法中，往这个watcher池中放对应属性的watcher。他的发布就是在set的时候，调用dep池中的watcher的notify方法去更新视图，这样就把代码组织起来并实现了双向的绑定

三种方式：

发布者-订阅者模式（backbone.js，比较 low），脏值检测（angular.js），数据劫持（vue.js）

vue 采用数据劫持配合“发布者-订阅者”模式来实现数据双向绑定。

数据变化更新视图，视图变化更新数据，通过数据劫持监听来实现。

\1. 设置监听器对 view 层数据进行劫持监听，发生变化时，通知订阅者们（watcher）更新数据

\2. 订阅者收到变化通知并执行相应函数，对数据重新赋值，改变 model 层

\3. 指令解析器对元素节点进行扫描和解析，根据指令模板替换数据

![img](http://note.youdao.com/yws/public/resource/493dfe1ab5c8c391fb978cf6701337e0/xmlnote/CD0F78F54FBF4923BAB5F56336167417/2151)


**95. vue 指令**

v-if，v-show，v-for，v-on，v-bind，v-html，v-model，v-once

**97. SPA 的优缺点**

> 优点：

> > 分离前后端关注点

> > 用户体验好，不需要加载页面

> > 充分利用缓存，减少服务器压力

> > 第一次请求中加载所有资源，不需要重新加载

> >

> 缺点：

> > SEO 无法抓取（没有 html 页面，而且使用路由，不发送请求）

> > 初始时加载比较慢，消耗客户端的性能

> > 用户操作前进后退，需要写逻辑来完成，不能用浏览器的返回按钮

> > 页面复杂度，难度提高

**99. VUE 有哪些好处？**

\1. 低耦合

\2. 可重用性

\3. 独立开发，前后端关注点分离

\4. 数据驱动界面，优化性能

**100. VUE 自定义组件的步骤**

第一步：在 components 目录新建你的组件文件（indexPage.vue），script 一定要 export default {}

第二步：在需要用的页面（组件）中导入：import indexPage from '@/components/indexPage.vue'

第三步：注入到 vue 的子组件的 components 属性上面,components:{indexPage}

第四步：在 template 视图 view 中使用

例如有 indexPage 命名，使用的时候则 index-page

可以结合回忆一下插件怎么用

**101. VUEX 属性**

> State、 Getter、Mutation 、Action、 Module

> vuex 的 State 特性

> > A、Vuex 就是一个仓库，仓库里面放了很多对象。其中 state 就是数据源存放地，对应于一般 Vue 对象里面的 data

> > B、state 里面存放的数据是响应式的，Vue 组件从 store 中读取数据，若是 store 中的数据发生改变，依赖这个数据的组件也会发生更新

> > C、它通过 mapState 把全局的 state 和 getters 映射到当前组件的 computed 计算属性中

> vuex 的 Getter 特性

> > A、getters 可以对 State 进行计算操作，它就是 Store 的计算属性

> > B、 虽然在组件内也可以做计算属性，但是 getters 可以在多组件之间复用
    
> > C、 如果一个状态只在一个组件内使用，是可以不用 getters

> vuex 的 Mutation 特性

> > Action 类似于 mutation，不同在于：Action 提交的是 mutation，而不是直接变更状态；Action 可以包含任意异步操作。

> 应用级的状态集中放在 store 中； 改变状态的方式是提交 mutations，这是个同步的； 异步逻辑应该封装在 action 中。

**102. 不使用 VUEX 会有哪些问题**

> 可维护性下降，修改一个数据要维护多个地方

> 可读性下降，一个组件里的数据看不出从哪里来的

> 增加了耦合性，大量的上传派发

**103. 你常用生命周期在哪些场景？**

> beforecreate：loading 事件

> created：结束 loading，及其他初始化完成时的事件，异步请求也可以在这里调用

> mounted：需要使用 DOM 元素的事件

> updated：对数据的统一处理

> beforedestroy：做一个确认框

> （nextTick：更新数据后立即操作 DOM）

**104. 说一下封装 vue 组件的过程**

> 使用 Vue.extend 方法创建一个组件，然后使用 Vue.component 方法注册组件。子组件需要数据，可以在 props 中接受定义。而子组件修改好数据后，想把数据传递给父组件。可以采用 emit 方法。

**105. 对 VUE 的 template 编译的理解**

​ compile 编译器将 template 转化为 AST（抽象语法树），对静态节点打上static的tag。合并 option，AST 经过 generate 得到 render 函数，返回值是 VNODE 虚拟 DOM 节点（包括标签名、子节点、文本等）。

**109. 自己写过组件么？描述一下**
简单的有一些按钮和模态框的封装，toast dailog confirm 封装，复杂点的有虚拟列表封装
封装分为两类，一个时函数式组件，一个时options的组件 函数式组件的特点是可以在函数运行过程中通过函数调用的方式在页面上渲染相应的带有逻辑功能的组件，常用的有toast和confirm组件这种，封装的方法是定义一个没有options的组件，通过Vue.extend方法去引入这个组件包装成一个class类，然后定义一个函数，在函数的内部实例化这个类并传入options配置项，然后将这个方法挂载到原型上，或者导出去通过Vue.use注册成插件，这样就可以在组件中通过this.$xxx来调用，或者在单纯的js文件中直接通过函数的形式调用
此外就是比较常规的options组件的封装，其核心是先设定好要预留哪些配置项，然后在组件中通过props去接收这些属性，如果dom结构也需要预留用户自定义的，则需要通过插槽来实现
完了之后可以通过正常的模块化的方式去引入使用，或者把他注册成全局组件Vue.component('name',xxx)

**113. elementUI 怎么扩展组件**

### 生命周期

+ vue组件的生命周期主要有四个阶段,创建=>挂载=>更新=>销毁,每个阶段有对应的生命周期钩子,beforeCreate,created,beforeMount,mounted,beforeupdate,updated,beforeDestory,destroyed.比较常用的是created和mounted,created阶段是最早的可以操作数据,可以在这个阶段发送ajax请求,monted阶段是dom已经挂载到页面上,在这个阶段才可以操作dom

### vuex

+ 为什么要用vuex不用全局变量
  - 避免数据污染，将全局状态通过单向数据流这种方式管理起来

### 为什么组件的data是用一个函数返回对象的形式

  + 个组件被创建好之后，就可能被用在各个地方，而组件不管被复用了多少次，组件中的data数据都应该是相互隔离，互不影响的，基于这一理念，组件每复用一次，data数据就应该被复制一次，之后，当某一处复用的地方组件内data数据被改变时，其他复用地方组件的data数据不受影响;如果直接是一个对象,所有的组件实例共用了一份data数据，因此，无论在哪个组件实例中修改了data,都会影响到所有的组件实例,而如果data不是一个单纯的对象，而是一个函数返回值的形式，所以每个组件实例可以维护一份被返回对象的独立拷贝.

  - 由于js的特性所致，存储数据的对象是复杂数据类型，需要存放在堆中，对一个对象的引用其实只是对该对象地址的引用而已。那么这样就会造成一个情况，假如某个对象的引用修改了这个对象上某个属性的值，就会导致在这个对象的其他引用上的该属性的值也会随之发生改变，这并不是我们想要的结果。

### 组件传值

+ props,emit,vuex,parent,children，eventBus（new vue，然后定义install方法，引入到入口中 use）provide/inject $attrs $listenersd
