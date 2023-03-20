# VUE3
笔记
脚手架文件结构
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   │── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件
关于不同版本的 Vue
vue.js 与 vue.runtime.xxx.js 的区别：
vue.js是完整版的Vue，包含：核心功能+模块解析器。
vue.runtime.xxx.js是运行版的Vue，只包含：核心功能；没有模板解析器。
由于vue.runtime.xxx.js没有模板解析器，所以不能使用template这个配置项，需要使用render函数接收到的createElement函数去指定具体内容。
vue.config.js配置文件
使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
使用vue.config.js可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh
参考属性
被用来给元素或子组件注册引用信息（id的替代者）
应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
使用方式：
打标识：<h1 ref="xxx">.....</h1>或<School ref="xxx"></School>
获取：this.$refs.xxx
道具配置项
功能：让组件接收外部传过来的数据

传讯资料：<Demo name="xxx"/>

接收数据：

第一种方式（只接收）：props:['name'] 

第二种方式（限制类型）：props:{name:String}

第三种方式（限制类型、限制必须性、指定默认值）：

props:{
	name:{
	type:String, //类型
	required:true, //必要性
	default:'老王' //默认值
	}
}
备注：props是只，vue底层底层对你你你的的的的的修改数据中的数据。

mixin(混入)
功能：可以把多个组件共用的配置提取成一个混入对象

使用方式：

第一步确定混合：

{
    data(){....},
    methods:{....}
    ....
}
第二步使用混合输入：

全部混入： 局Vue.mixin(xxx) 混入：mixins:['xxx']	

插件
功能：用于增强Vue

本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

定义插件：

对象.install = function (Vue, options) {
    // 1. 添加全局过滤器
    Vue.filter(....)

    // 2. 添加全局指令
    Vue.directive(....)

    // 3. 配置全局混入(合)
    Vue.mixin(....)

    // 4. 添加实例方法
    Vue.prototype.$myMethod = function () {...}
    Vue.prototype.$myProperty = xxxx
}
使用插件：Vue.use()

范围样式
作 用：让样式在局部产生效果，防止冲突。
写法：<style scoped>
总结TodoList案例
组件化编码流程：

(1).拆分静态组件：组件要按功能点拆分，名称不要与html元素冲突。

(2).实际动态组：考虑好数据的存储位置，数据是一个组在用，还是一些组在用：

1). 一个正在使用的组件：放在组件本身即可。

2). 一些组在用：放在他们共同的父组上（状态提升）。

(3).现实交际：从绑定事件开始。

道具适用于：

(1).父亲组 ==> 子组通信

(2).子组 ==> 父组通信（要父先给子一个函数）

使用v-model时要切记：v-model绑定的价值不是props传过来的价值，因为props是无法修改的！

props传过来的若是对图像类型的价值，修改对图像中的属性时Vue不会报错，但不推荐这样做。

网络存储
存储内容大一般支持5MB左右（不同浏览器可能还是不一样）

浏览器端通过 Window.sessionStorage 和 Window.localStorage 属性来实现本地存储机制。

相关API：

xxxxxStorage.setItem('key', 'value'); 该方法接受一个键和值作为参数，会把键值添加到存储中，如果键名存在，则更新其对应的值。

xxxxxStorage.getItem('person');

该方法接受一个键名作为参数，返回键名对应的值。

xxxxxStorage.removeItem('key');

该方法接受一个关键字作为参数，并把关键字从存储中删除。

 xxxxxStorage.clear()

该方法会清空存储中的所有数据。

备 注：

SessionStorage存储的内容会随着浏览器窗口关闭而消失。
LocalStorage存储的内容，需要手动清除才会消失。
xxxxxStorage.getItem(xxx)如果xxx对应的值获取不到，那么getItem的返回值为null。
JSON.parse(null)结果依然是null。
组的自定义事件
一种组合件间通信的方式，适用于：子组合件 ===> 父组合件

使用场景：A是父组，B是子组，B想给A数据，那么只要在A中给B绑定自定义事件（事件的回调在A中）。

绑定自定义事件：

第一种方式，在父组中：<Demo @atguigu="test"/> 或<Demo v-on:atguigu="test"/>

第二种方式，在父组中：

<Demo ref="demo"/>
......
mounted(){
   this.$refs.xxx.$on('atguigu',this.test)
}
若想让自定义事件只能触发一次，可以使用once修改符，或$once方法。

触发自定义事件：this.$emit('atguigu',数据)

解绑自定义事件this.$off('atguigu')

组上也可以绑定原始DOM事件，需要使用native修饰符。

注意：通过this.$refs.xxx.$on('atguigu',回调)绑定自定义事件时，返回调或者配置在方法中，或者用箭头函数，否则这个指标会出问题！

全局事件总线（GlobalEventBus）
一种组间通信的方式，适用于任组间通信。

装修公司总行：

new Vue({
	......
	beforeCreate() {
		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
	},
    ......
}) 
使用事件总线：

接收数据：A组想接收数据，则在A组中给$bus绑定自定义事件，事件的回复留在A组自身。

methods(){
  demo(data){......}
}
......
mounted() {
  this.$bus.$on('xxxx',this.demo)
}
提供资料：this.$bus.$emit('xxxx',数据)

最好在beforeDestroy锁子中，用$off去解绑当组合件用到的事。

消息订阅与发布（pubsub）
一种组间通信的方式，适用于任组间通信。

使用步骤：

安装pubsub：npm i pubsub-js

引入：import pubsub from 'pubsub-js'

接收数据：A组想接收数据，则在A组中订阅消息，订阅的回调留在A组自身。

methods(){
  demo(data){......}
}
......
mounted() {
  this.pid = pubsub.subscribe('xxx',this.demo) //订阅消息
}
提供资料：pubsub.publish('xxx',数据)

最好在beforeDestroy锁子中，PubSub.unsubscribe(pid)用去取消订阅。

下一步
语法：this.$nextTick(回调函数)
作用：在下一次DOM更新结束后执行其指定的回调。
什么时候使用：当修改数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。
Vue封装的过度与动画
作用：在插入、更新或移去DOM元素时，在合适的时候给元素添加样式类名。

图片说明：

写法：

准备好样式：

元素进入的样式：
v-enter：进入的起点
v-enter-active：进入过程中
v-enter-to：进入的终点
元素分离的样式：
v-leave：离开的起点
v-leave-active：离开进程中
v-leave-to：离开的终点
使用<transition>包裹要过度的元素，并配置名称属性，注意如果配置了出现属性的对话就代表一开始挂载真实dom的时候就开启动画的效果：

<transition name="hello" appear>
	<h1 v-show="isShow">你好啊！</h1>
</transition>
备 注：若有多个元素需要过度，则需要使用：<transition-group>，并且每个元素都需要指定key值。

vue脚手架配置代理
方法一
在vue.config.js中添加如下配置：

devServer:{
  proxy:"http://localhost:5000"
}
说明：

优点：配置简单，请请求资源时直接发送给前端（8080）即可。
不足之处：不能配置多个代理，不能精神生活的控制请求是否走代理。
工作方式：若按照片上描述的配置代理，当请求了前端不存在的资源时，那么应该请求会转发服务器（优先匹配前端资源）
方法二
编写vue.config.js配置工具代理规则：

module.exports = {
	devServer: {
      proxy: {
      '/api1': {// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api1': ''}
      },
      '/api2': {// 匹配所有以 '/api2'开头的请求路径
        target: 'http://localhost:5001',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      }
    }
  }
}
/*
   changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
   changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
   changeOrigin默认值为true
*/
说明：

优点：可以配置多个代理，并且可以灵魂生活的控制请求是否走代理。
不足之处：配置略微繁琴，请寻求资源时必须加前置。
插槽
作用：让父组可以向子组指定位置插入html结构，也是一种组间通信的方式，适用于父组 ===> 子组。

分类：默认插件、具名插件、作用域插件

使用方式：

默认插槽：

父组件中：
        <Category>
           <div>html结构1</div>
        </Category>
子组件中：
        <template>
            <div>
               <!-- 定义插槽 -->
               <slot>插槽默认内容...</slot>
            </div>
        </template>
工具名称插孔：

父组件中：
        <Category>
            <template slot="center">
              <div>html结构1</div>
            </template>

            <template v-slot:footer>
               <div>html结构2</div>
            </template>
        </Category>
子组件中：
        <template>
            <div>
               <!-- 定义插槽 -->
               <slot name="center">插槽默认内容...</slot>
               <slot name="footer">插槽默认内容...</slot>
            </div>
        </template>
使用域插件：

理解：数据在自身，但但根据根据生成结构结构组件组件的使用者来来决定决定决定。。。。。（。。（数据数据数据数据数据数据数据数据组件组件组件组件组件

具体代码：

父组件中：
		<Category>
			<template scope="scopeData">
				<!-- 生成的是ul列表 -->
				<ul>
					<li v-for="g in scopeData.games" :key="g">{{g}}</li>
				</ul>
			</template>
		</Category>

		<Category>
			<template slot-scope="scopeData">
				<!-- 生成的是h4标题 -->
				<h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
			</template>
		</Category>
子组件中：
        <template>
            <div>
                <slot :games="games"></slot>
            </div>
        </template>
		
        <script>
            export default {
                name:'Category',
                props:['title'],
                //数据在子组件自身
                data() {
                    return {
                        games:['红色警戒','穿越火线','劲舞团','超级玛丽']
                    }
                },
            }
        </script>
Vuex
1.概念
vue中中集中式状态（数据状态）vue插件任意组间通信。

2.什么时候使用？
多个组件需要共享数据时

3.搭建vuex环境
创建文件：src/store/index.js

//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)

//准备actions对象——响应组件中用户的动作
const actions = {}
//准备mutations对象——修改state中的数据
const mutations = {}
//准备state对象——保存具体的数据
const state = {}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state
})
在main.js中创建vm时传入store配置项

......
//引入store
import store from './store'
......

//创建vm
new Vue({
	el:'#app',
	render: h => h(App),
	store
})
4.基础使用
初始化数据、配置actions、配置mutations，操作文件store.js

//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//引用Vuex
Vue.use(Vuex)

const actions = {
    //响应组件中加的动作
	jia(context,value){
		// console.log('actions中的jia被调用了',miniStore,value)
		context.commit('JIA',value)
	},
}

const mutations = {
    //执行加
	JIA(state,value){
		// console.log('mutations中的JIA被调用了',state,value)
		state.sum += value
	}
}

//初始化数据
const state = {
   sum:0
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
组件中读取vuex中的数据：$store.state.sum

组件中修改vuex中的数据：$store.dispatch('action中的方法名',数据)或者$store.commit('mutations中的方法名',数据)

备 注：若没有网络请求或其他业务通讯，组件中也可以越过actions，即不写dispatch，直接编辑commit

5.getters的使用
概念：当state中的数据需要经过加工后再次使用时，可以使用getters加工。

在store.js中追加getters配置

......

const getters = {
    bigSum(state){
        return state.sum * 10
    }
}

//创建并暴露store
export default new Vuex.Store({
    ......
    getters
})
组件中读取数据：$store.getters.bigSum

6.四个地图方法的使用
mapState方法：用于帮助我们映射state中的数据为计算属性

computed: {
    //借助mapState生成计算属性：sum、school、subject（对象写法）
     ...mapState({sum:'sum',school:'school',subject:'subject'}),
         
    //借助mapState生成计算属性：sum、school、subject（数组写法）
    ...mapState(['sum','school','subject']),
},
mapGetters方法：用于帮助我们映射getters中的数据为计算属性

computed: {
    //借助mapGetters生成计算属性：bigSum（对象写法）
    ...mapGetters({bigSum:'bigSum'}),

    //借助mapGetters生成计算属性：bigSum（数组写法）
    ...mapGetters(['bigSum'])
},
mapMutations方法：用于帮助我们生成与mutations对话的方法，即：包含$store.commit(xxx)的函数

methods:{
    //靠mapActions生成：increment、decrement（对象形式）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
    
    //靠mapMutations生成：JIA、JIAN（对象形式）
    ...mapMutations(['JIA','JIAN']),
}
备注：mapActions与mapMutations使用时，若需要传参数需要：在模板中绑定事件时传传好的参数，否则传参数是事件对象。

7.模块化+生命名空间
目的的：让代码更好维护，让多种数据分类更明确。

修改store.js

const countAbout = {
  namespaced:true,//开启命名空间
  state:{x:1},
  mutations: { ... },
  actions: { ... },
  getters: {
    bigSum(state){
       return state.sum * 10
    }
  }
}

const personAbout = {
  namespaced:true,//开启命名空间
  state:{ ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    countAbout,
    personAbout
  }
})
打开启动名称空间后，组件中读取状态数据：

//方式一：自己直接读取
this.$store.state.personAbout.list
//方式二：借助mapState读取：
...mapState('countAbout',['sum','school','subject']),
打开启动名称空间后，组件中读取getters数据：

//方式一：自己直接读取
this.$store.getters['personAbout/firstPersonName']
//方式二：借助mapGetters读取：
...mapGetters('countAbout',['bigSum'])
启动名称空间后，组件中调使用dispatch

//方式一：自己直接dispatch
this.$store.dispatch('personAbout/addPersonWang',person)
//方式二：借助mapActions：
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
启动命名空间后，组件中调使用commit

//方式一：自己直接commit
this.$store.commit('personAbout/ADD_PERSON',person)
//方式二：借助mapMutations：
...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
·由
理解：一条路由（route）就是一组映射关系（key-value），多条路由需要路由器（router）进行管理。
前端路径由：key是路径，value是组件。
1.基础使用
安装vue-router，命令：npm i vue-router

应用插件：Vue.use(VueRouter)

编写router配置项：

//引入VueRouter
import VueRouter from 'vue-router'
//引入Luyou 组件
import About from '../components/About'
import Home from '../components/Home'

//创建router实例对象，去管理一组一组的路由规则
const router = new VueRouter({
	routes:[
		{
			path:'/about',
			component:About
		},
		{
			path:'/home',
			component:Home
		}
	]
})

//暴露router
export default router
实际切换（active-class可配置高亮样式）

<router-link active-class="active" to="/about">About</router-link>
指定展示位置

<router-view></router-view>
2.几个注意点
由组通常存储在pages文件夹中，一般组通常存储在components文件夹中。
通过切换，“隐藏”了的路由组件，默认是被营销损坏的，需要的时候再去挂载。
每个组都有自己的$route属性，里面存储着自己的路由信息​​。
整个应用程序只有一个路由器，可以通过组件的$router属性获取到。
3.多级路由（多级路由）
配置路径由规则，使用儿童配置项：

routes:[
	{
		path:'/about',
		component:About,
	},
	{
		path:'/home',
		component:Home,
		children:[ //通过children配置子级路由
			{
				path:'news', //此处一定不要写：/news
				component:News
			},
			{
				path:'message',//此处一定不要写：/message
				component:Message
			}
		]
	}
]
跳转（要写完整路径）：

<router-link to="/home/news">News</router-link>
4.路由的查询参数
传递参数

<!-- 跳转并携带query参数，to的字符串写法 -->
<router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>
				
<!-- 跳转并携带query参数，to的对象写法 -->
<router-link 
	:to="{
		path:'/home/message/detail',
		query:{
		   id:666,
            title:'你好'
		}
	}"
>跳转</router-link>
接收人数：

$route.query.id
$route.query.title
5.命名路由
作用：可以简单化路径由跳转。

如何使用

给路由命名：

{
	path:'/demo',
	component:Demo,
	children:[
		{
			path:'test',
			component:Test,
			children:[
				{
                      name:'hello' //给路由命名
					path:'welcome',
					component:Hello,
				}
			]
		}
	]
}
简化跳转：

<!--简化前，需要写完整的路径 -->
<router-link to="/demo/test/welcome">跳转</router-link>

<!--简化后，直接通过名字跳转 -->
<router-link :to="{name:'hello'}">跳转</router-link>

<!--简化写法配合传递参数 -->
<router-link 
	:to="{
		name:'hello',
		query:{
		   id:666,
            title:'你好'
		}
	}"
>跳转</router-link>
6.路由的参数参数
配置路径由，声明接收params参数

{
	path:'/home',
	component:Home,
	children:[
		{
			path:'news',
			component:News
		},
		{
			component:Message,
			children:[
				{
					name:'xiangqing',
					path:'detail/:id/:title', //使用占位符声明接收params参数
					component:Detail
				}
			]
		}
	]
}
传递参数

<!-- 跳转并携带params参数，to的字符串写法 -->
<router-link :to="/home/message/detail/666/你好">跳转</router-link>
				
<!-- 跳转并携带params参数，to的对象写法 -->
<router-link 
	:to="{
		name:'xiangqing',
		params:{
		   id:666,
            title:'你好'
		}
	}"
>跳转</router-link>
特别注意：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！

接收人数：

$route.params.id
$route.params.title
7.路由的道具配置
作用：让路由组合件更方便的收集到参数

{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,

	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
	// props:{a:900}

	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
	// props:true
	
	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
	props(route){
		return {
			id:route.query.id,
			title:route.query.title
		}
	}
}
8.<router-link>的替换属性
作 用：控制路由跳转时操作作浏览器历史记录的模式
浏览器的历史记录有两种写入方式：分别为push和replace，push是追加历史记录，replace是替代当前记录。路由跳转时默认为push
如何开启replace模式：<router-link replace .......>News</router-link>
9.编程方式由导航
作 用：不借帮助<router-link> 实际现路由跳转，让路由跳转更加灵活

具体代码：

//$router的两个API
this.$router.push({
	name:'xiangqing',
		params:{
			id:xxx,
			title:xxx
		}
})

this.$router.replace({
	name:'xiangqing',
		params:{
			id:xxx,
			title:xxx
		}
})
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go() //可前进也可后退 参数是number类型(正数为前进的步数,负数为后退的部署)
10.存储路径由组件
作 用：让不显示的路组组件保持挂载，不被销售毁坏。

具体代码：

<keep-alive include="News"> 
    <router-view></router-view>
</keep-alive>
11.两个新的生命周锁子
作 用：路由组所独有的两个钩子，用于捕捉路由组的活跃状态。
具体名称：
activated·由组件被激动时触发。
deactivated·路由组合件失活时触发。
12.路由守卫
作 用：对路由进入行权限制控制

分类：全局守卫、独享守卫、组内守卫

全局守卫：

//全局前置守卫：初始化时执行、每次路由切换前执行
router.beforeEach((to,from,next)=>{
	console.log('beforeEach',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
		if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则
			next() //放行
		}else{
			alert('暂无权限查看')
			// next({name:'guanyu'})
		}
	}else{
		next() //放行
	}
})

//全局后置守卫：初始化时执行、每次路由切换后执行
router.afterEach((to,from)=>{
	console.log('afterEach',to,from)
	if(to.meta.title){ 
		document.title = to.meta.title //修改网页的title
	}else{
		document.title = 'vue_test'
	}
})
独享守卫:

beforeEnter(to,from,next){
	console.log('beforeEnter',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
		if(localStorage.getItem('school') === 'atguigu'){
			next()
		}else{
			alert('暂无权限查看')
			// next({name:'guanyu'})
		}
	}else{
		next()
	}
}
组内卫：

//进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter (to, from, next) {
},
//离开守卫：通过路由规则，离开该组件时被调用
beforeRouteLeave (to, from, next) {
}
13.路由器的两种工作模式
对于一个url来说，什么是hash值？——#及其后面的内容就是hash值。
hash值不会包含在HTTP请求中，即：hash值不会带给服务器。
哈希模型：
地址中永远带着#号，不美观。
若以后将地址通过第三方手机app分享，若app校园严格格式，则地址会被标记为不合法。
包容性比较好。
历史模式：
地干净，美观。
兼顾性和散列模型相比较略差。
应用部在线时需要后端人员支持，解决刷新页面服务端404的问题。
