

# p1







# p2

```html
<script>
	const app = new Vue({
        el:"#app",   //element 用来给Vue实例定义一个作用范围
        data:{		 //用来给Vue实例定义一些相关数据
            msg:"xxx"
        }
    })


</script>
```

### v-text

以`文本`的形式渲染数据

> 使用`插值表达式`和`v-text`的区别在于
>
> a. 使用v-text取值会将标签中原有的数据覆盖
>
> b. 使用`v-text`可以避免在网络环境比较差的情况下出现插值闪烁

### v-html

将数据中含有的HTML标签先解析再渲染到指定标签内部，类似于JavaScript中的`innerHtml`





# p3

### v-on  事件绑定

```html
<div> {{ age }} </div>
</div>		
<div v-on:click="clickMethod(24)">   <!-- 或者用@click也可以 -->
    按钮
</div>

<script>
	const app  = new Vue({
        el:"#app",
        data:{
            age:18,
        },
        methods:{
            clickMethod(age){
                // 在函数中获取vue实例中data的数据，在事件函数中this就是vue实例
                this.age = age;  //参数传递
            }
        }
    })

</script>

```

> :evergreen_tree:事件    `事件源:发生事件dom元素`      `事件：发生特定的动作`      `监听器：发生特定动作之后的事件处理程序，通常是js中函数`

> :yellow_heart:用 `@事件` 去替代 `v-on:事件`  比较方便





# p4

### v-show

> `v-show` 用来控制页面中某个元素是否显示，底层是元素的`display`属性，如果元素需要频繁的切换的话效率会比较高





### v-if

> `v-if`  底层通过对dom树节点进行添加和删除来控制展示和隐藏

> :tangerine:  通常用`:事件` 来替换 `v-bind:事件`



### v-bind

> `v-bind`  用来给页面中标签元素绑定相应的属性







# p5

### v-for

> `v-for`用来对对象（数组也是对象的一种）进行遍历

```vue
<template>
	<div id="app">
        <!-- 遍历对象 -->
        <span v-for="(value,key,index) in user">
            {{ index }} : [{{ key }} | {{ value }}]
   		</span>
        
        <!-- 遍历数组 -->
        <span v-for="(value,index) in arr">
    		{{ value }} | {{ index }}
    	</span>

        
        
        <!-- 遍历数组中的对象 -->
        <span v-for="u in users">
    		{{ u.name }}  |  {{ u.age }}
    	</span>
    </div>

</template>

<script>
	const app = new Vue({
        el:'#app',
        data:{
            user:{
                name:"张三",
                age:18
            },
            arr:[
                '数组1',
                '数组2',
                '数组3'
            ],
            users:[
                {name:'张三',age:18},
                {name:'李四',age:20}
            ]
        }
    })
</script>
```





> 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而**重用**和重新**排序**现有元素，你需要为每项提供一个唯一 `key` attribute：

```vue
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```

> 建议尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
>
> :warning:不要使用**对象**或**数组**之类的非基本类型值作为 `v-for` 的 `key`。请用字符串或数值类型的值。





### 双向绑定

```vue
<template>
	<div id="app">
       <input type="text" v-model="message"/> 
     	<span v-text="message"></span>
    </div>
	
</template>


<script>
	const app = new Vue({
        el:'#app',
        data:{
            message:''
        }
    })

</script>


```



> :apple:`双向绑定`：表单中数据变化导致vue实例data数据变化，vue实例中data数据变化导致表单中数据变化





> :banana: **MVVM架构**
>
> `Model`  数据  vue实例中绑定数据
>
> `VM`  ViewModel  监听器   实时监听View和Model的变化
>
> `View`  页面   页面展示的数据

  





# p8

### 事件修饰符

> `修饰符` 用来和事件连用个，决定事件触发条件或是阻止事件的触发机制 

#### 1.stop

> 阻止事件冒泡

```vue
<template>
	<div @click="divClick">
        <!-- 在事件后面添加.stop即可阻止事件向上一层冒泡 -->
        <button @click.stop="buttonClick">按钮</button> 
    </div>

</template>

<script>
	const app = new Vue({
        methods:{
            btnClick(){
                alert('button被单击了')
            },
            divClick(){
                alert('div被点击了')
            }
        }
    })

</script>


```



#### 2.prevent

> 阻止标签的默认行为

```vue
<template>
	<a href="www.baidu.com" @click.prevent="aClick">我是百度的超级链接</a>
</template>

<script>
	const app = new Vue({
        methods:{
            aClick(){
                alert('a标签被点击了')
            }
        }
    })

</script>
```



#### 3.self

> 只关心自己标签上触发的事件，不再监听事件冒泡

> :tipping_hand_man:如果使用`.stop`事件修饰符的话，需要添加很多个`@click.stop` 这个时候在父标签用 `.self`就容易多了

```vue
<template>
	<div id="app">
        <div @click.self="divClick">
            <button @click="btnClick">按钮</button>
            <button @click="btnClick">按钮</button>
            <button @click="btnClick">按钮</button>
            <button @click="btnClick">按钮</button>
    	</div>
    </div>
</template>
```



#### 4.once

> :one: 让指定的事件只触发一次 
>
> :tipping_hand_man: 可以和其他的事件修饰符`.prevent` 一起使用

```vue
<template>
	<a href="www.baidu.com" @click.prevent.once="aClick">
		百度    
    </a>
</template>
```





# p9

### 按键修饰符

> :zap: 用来与键盘中按键事件绑定在一起，用来修饰特定的按键事件的修饰符

#### 1.enter

> :arrow_right_hook: 在回车按键之后触发的事件

```vue
<template>
	<input type="text" v-model="msg" @keyup.enter="enterKeyup" />
	<button @click="enterKeyup">
         搜索
    </button>
</template>

<script>
	const app = new Vue({
        methods:{
            data:{
              msg:""  
            },
            enterKeyup(){
                console.log(this.msg);
            }
        }
    })

</script>
```





#### 2.tab

> :cowboy_hat_face: 跟上面的用法差不多，在别的输入框 `tab` 转换焦点的时候触发



#### 3.esc





# p10

### axios

#### 安装

```js
npm install axios --save  ???
```



#### 并发请求

> `并发请求`   将多个请求在同一时刻发送到后端接口，最后集中处理每个请求的响应结果

```js
function find(){
    return axios.get('/api/find');
}

function save(){
    return axios.post('/api/save',{
        name:'zhangsan',
        age:18
    });
}

//并发执行
axios.all([find(),save()]).then(  //发送一组请求，集中处理结果
	axios.spread( (res1,res2) => {   //对结果进行汇总处理
        console.log(res1);
        console.log(res2);
    })	
);
```







# p12

### 生命周期钩子

> :warning:  生命周期钩子 =  生命周期函数

<img src="D:\study data\学习笔记\Vue\Vue学习笔记.assets\image-20200508101139107.png" alt="image-20200508101139107" style="zoom:50%;" />



> :one:  初始化阶段

`beforeCreate`  生命周期中第一个函数，该函数在执行时Vue实例仅完成自身事件的绑定和生命周期函数的初始化工作，Vue实例中还没有**data**、**el**、**methods** 相关属性。



`created`  生命周期中第二个函数，该函数在执行时Vue实例已经初始化**data**属性和**methods**中相关方法。



`beforeMount`  生命周期中第三个函数，该函数在执行时Vue将**el**中指定作用范围作为模板编译



`mounted`  生命周期第四个函数，该函数在执行过程中，已经将数据渲染到界面并且已经更新界面







> :two:  运行阶段

`beforeUpdate `  生命周期第五个函数，该函数在**data**数据发生变化时执行，这个事件执行时仅仅只是Vue实例中**data**数据变化，页面依然显示的是原始的数据



`updated`   生命周期第六个函数，该函数执行时**data**数据发生变化，页面中数据已经和**data**的数据一致



> :three:  销毁阶段

`beforeDestroy`  生命周期第七个函数，该函数执行时，Vue中所有数据，`methods`、`component`都没有销毁。



`destroyed`  生命周期第八个函数，该函数执行时，Vue实例彻底销毁





# p23

### 组件的作用

>  :tipping_hand_man: 用来减少Vue实例对象中代码量，日后在使用Vue开发过程中，可以根据不同业务将页面划分成不同组件，然后由多个组件去完成整个页面的布局，便于日后使用Vue进行开发时页面管理，方便开发人员维护。

### 组件的使用

#### 全局组件注册

>  :tipping_hand_man: 全局组件注册给Vue实例，日后可以在任意Vue实例范围内使用该组件

```vue
//开发全局组件
Vue.component('login',{
	template:'<div>
        <h1>
            用户登陆
        </h1>
</div>'
})

//使用全局组件
<login></login>
```

> `Vue.component` 用来开发全局组件
>
> `参数1`：组件的名称
>
> `参数2`：组件配置  `template` 用来书写组件的html代码，`template` 中必须只能有唯一一个root对象

> 如果在注册组件的过程中使用**驼峰命名**组件的方式，在使用组件需要在单词前加入**-**

#### 局部组件注册

>  :tipping_hand_woman: 通过将组件注册给对应Vue实例中一个`components`属性来完成组件注册，这种方式不会导致`Vue实例`过于臃肿

- 第一种方式

```vue
<script>
	let login = {
        template:'<div><h1>登陆页面</h1></div>'
    }
    
    const app = new Vue({
        el:'#app',
        components:{
            login:login  // 注册局部组件，如果注册名称与组件名称相同可不用给其命名
        }
    })
</script>
```

- 第二种方式

```vue
//声明局部组件模板
<template id="login"></template>


<script>
	//定义变量用来保存模板配置对象
    let login = {
        template:'#login'
    }
    
    //注册组件
    const app = new Vue({
        el:'#app',
        components:{
            login
        }
    })
    
</script>

//局部组件使用
<login></login>
```





# p24

### props的使用

> `props` 用来给组件传递响应静态数据或动态数据

```vue
<div id="app">
    <login :user-name="username"></login>
    
    <register name="张三"></register>
</div>

<script>
    // 动态传递
	let login = {
        template:'<div><h1>欢迎:{{name}}</h1></div>',
        props:['username'],
        data(){  // 组件中的data数据
            return {
                name:this.username  
            }
        }
    }
    
    // 静态传递
    let register = {
        template:'<div><h1>注册名：{{ username }}</h1></div>',
        props:['username']
    }
    
    const app = new Vue({
        el:'#app',
        data:{
            username:"小黑"
        },
        methods:{},
        components:{
            login //注册组件
            ,register
        }
    })
</script>
```



# p25

### 组件中事件的使用

```vue
<div id="app">
    <login></login>
    
</div>

<script>
    // 动态传递
	let login = {
        template:'<div><h1 @click="change">欢迎</h1></div>',
        methods:{
            change(){
                alert('h1被点击了')
            }
        }
    }
    
    const app = new Vue({
        el:'#app',
        data:{
            username:"小黑"
        },
        methods:{},
        components:{
            login //注册组件
           
        }
    })
</script>
```



# p26

### 组件事件的传递

> 在子组件中调用传递过来的相关时间必须使用 `this.$emit('函数名')` 的方式

```vue
<div id="app">
    <!--使用组件，传递事件-->
    <login @vueAlert="vueAlert"></login>
</div>

<script>
    //声明组件
	let login = {
        template:'<div><h1 @click="componentsAlert">欢迎</h1></div>',
        methods:{
            componentsAlert(){
                alert("通过components实例弹窗");
                this.$emit('vueAlert');  //子组件调用父组件的方法
            }
        }
    }
    
    //注册组件
    const app = new Vue({
        el:'#app',
        data:{
            username:"小黑"
        },
        methods:{
            vueAlert(){
                alert("通过vue实例弹窗");
            }
        },
        components:{
            login //注册组件
        }
    })
</script>
```



> 子组件向父组件传递

```vue
<template>
	<section>
    	<h1 @click="childClick">子组件</h1>
    </section>
</template>

<script>
	export default{
        name:'child',
        methods:{
            childClick(){
                this.$emit('childEvent',now())
            }
        }
    }
</script>

========================================

<template>
	<section>
    	<child @childEvent="fatherListener"></child>
    </section>
</template>

<script>
    import child from 'child'
    
	export default{
        name:'child',
        component:{
            child
        },
        methods:{
            fatherListener(data){
                console.log(data) // 控制台打印2020年5月25日20:13:25
            }
        }
    }
</script>
```



[父组件怎么传值给子组件的v-modal]:"https://www.cnblogs.com/lundin/p/10749803.html"

# p27

### 路由

> `router` 根据请求的路径按照一定的路由规则进行请求的转发从而实现我们统一请求的管理

> **作用：**用来在vue中实现组件之间的动态切换

> **创建路由步骤**
>
> - 引入路由
>
> - 创建组件对象，定义路由对象的规则
>
>   ```js
>   const login ={
>       template:'<h1>登陆</hi>'
>   };
>   const register ={
>       template:'<h1>注册</hi>'
>   }
>   
>   const router = new VueRouter({
>       routers:[
>       { path:'/',redirect:'/login'}
>       { path:'/login',component:login },
>       { path:'/register',component:register }
>       ]
>   })
>   ```
>
> - 将路由对象注册到vue实例
>
> ```js
>  const app = new Vue({
>      el:'#app',
>      router:router
>  })
> ```
>
> - 在页面中显示路由的组件
>
> ```vue
> <router-view></router-view>
> ```





# p28

### router-link使用

> **作用：**用来替换切换路由时候使用`<a></a>`标签切换路由
>
> **好处：**可以自动给路由路径加入`#`不需要去手动加入

> `tag` 用来替换渲染的标签

```html
<router-link to="/login" tag="button"></router-link>
```



### 默认路由

> :helicopter:  用来在第一次进入界面显示的一个默认组件

```js
const router = new VueRouter({
    routes:[
        {path:'/',redirect:'/login'},  //←默认路由
        {path:'/login',component:login},
        {path:'/register',component:register}
    ]
})
```



# p30

### 路由中的参数传递

- 通过`?`传递参数

```html
<router-view to="/login?username=zhangsan"></router-view>
```

> :ideograph_advantage: 获取

```js
const login = {
    template:'<h1>用户登录</h1>'，
    created(){
        let username = this.route.query.username;
    }
}
```



- `restful` 方式传递

```html
<router-link to="/login/23/zhangsan"></router-link>

<script>
	const router = new VueRouter({
        routes:[
            {path:'/login/:id/:name',component:login}
        ]
    })

</script>
```

> :ideograph_advantage:  获取

```js
const login = {
    template:'<h1>登录</h1>',
    created(){
        let username = this.$route.params.username;
        let id = this.$route.params.id;
    }
}
```





# p31

### 嵌套路由

> 路由中包含`子路由`

```js
const router = new VueRouter({
    {
    	path:'/login',
    	component:login,
    	children:[
    		{path:'add',component:add},
            {path:'edit',component:edit}
    	]
	}
})
```



# p33

### vue-cli

> **优势** 
>
> - 通过`vue-cli`搭建交互式项目脚手架，能够通过执行命令下载相关的依赖
>
> - 通过`@vue/cli` + `@vue/cli-service-global ` 快速开始零配置原型开发
> - 运行时依赖 `@vue/cli-service`
>   - 包容易升级
>   - `webpack`构建，带有合理的默认配置，通过打包可以很方便将编译好的源码部署到服务器
>   - 配置项目内配置文件达到想要的项目环境
>   - 插件扩展，如`v-charts`，`element-ui`等
> - 丰富的官方插件集合，集成了前端生态中最好的工具 `nodeJs` `Vue` `Vue-router` `webpack` `yarn`

> **安装**

```markdown
# 1.下载nodeJs
	http://nodejs.cn/download
# 2.配置nodeJs环境
	NODE_HOME = nodeJs安装目录
	PATH = xxxx;%NODE_HOME%
# 3.验证nodeJs安装是否成功
	node -v
# 4.npm (node package manager) 类似Maven
	npm config set registry https://registry.npm.taobao.org
	npm config get registry
# 5.配置npm依赖下载位置
	npm config set cache "d:\npm\npm-cache"
	npm config set prefix "d:\npm\npm-global"
# 6.查看配置环境
	npm config ls
# 7.卸载vue-cli
	npm uninstall -g @vue/cli   //卸载3.x
	npm uninstall -g vue-cli    //卸载2.x
# 7.安装vue-cli
	npm install -g vue-cli
```



# p35

### 创建vue-cli

```js
vue init <template-name> <project-name>
```

> :cowboy_hat_face:   **项目结构**
>
> <project-name> `项目名`
>
> ​	-build `webpack打包依赖`
>
> ​	-config `项目配置目录`
>
> ​	-node_modules  `项目管理依赖`
>
> ​	-src  
>
> ​		+assets `静态资源`
>
> ​		+components  `vue组件`
>
> ​		+router `路由`
>
> ​		+App.vue `根组件`
>
> ​		+main.js `主入口`
>
> ​	-static `其他静态资源`
>
> ​	-.babelrc `将es6语法转化为es5运行`
>
> ​	-.editorconfig  `项目编辑配置`
>
> ​	-.gitignore  `git忽略文件`
>
> ​	-.postcssrc.js  `源码相关js`
>
> ​	-index.html  `项目主页`
>
> ​	-package.json	`类似maven的pom.xml`
>
> ​	-package-lock.json  `对package.json加锁`
>
> ​	-README.md

> :hammer:  **打包**和**部署**

```markdown
npm run build
```







# Vuex

> 组件之间的数据共享

- 父向子：`v-bind`属性绑定
- 子向父：`v-on` 事件绑定
- 兄弟组件之间：`EventBus`
- `$on`  接收数据的那个组件
- `$emit`  发送数据的那个组件

### 基础概念
> `Vuex` 是实现组件**全局**状态（数据）管理的一种机制，可以方便的实现组件间的数据共享

> `Vuex` 的统一管理状态的好处

- 能够在`vuex`中集中管理共享的数据
- 高效的实现组件之间的数据共享
- 存储在`vuex`中的数据都是响应式的，能够实时保持数据与页面的同步

> 什么数据适合存储到`vuex`中

组件之间**共享**的数据，才有必要存储到`vuex`中，**私有**数据在组件自身的`data`存储。





### 核心概念

>  **state**

- 组件访问state数据的**第一种**方式

``` vue
<template>
    <div>
        <h1>
        	{{ $store.state.count }}    
    	</h1>
    </div>

</template>
    

const store = new Vuex.Store({
	state:{
		count: 0
	}
})  
```



- 组件访问state数据的**第二种**方式

```vue
<template>
	<div>
        <h1>
        	{{ count }}    
    	</h1>
    </div>
</template>

<script>
import { mapState } from 'vuex'
    
 export default {
     data(){
         return {}
     },
     computed:{
         ...mapState(['count'])
     }
 }
</script>
```



> **mutations**

> `mutation` 用于变更 `store`中的数据
>
> - 只能通过`mutation`变更`store`数据，不可直接操作`store`数据
> - 过程繁琐，但是可以集中**监控**所有数据的变化

- 调用函数的第**一**种方式

```js
// 定义mutation
const store = new Vuex.Store({
    state:{
        count:0
    },
    mutations:{
        add(state){
            state.count++
        }
    }
})


// 调用
methods:{
    handler(){
        // 通过mutation触发函数
        this.$store.commit('add')
    }
}
```



> :face_with_thermometer:  传递参数

```js
const store = new Vuex.Store({
    state:{
        count:0
    },
    mutations:{
        addN(state,step){
            state.count += step;
        }
    }
})

methods:{
    addN(){
        this.$store.commit('addN',3)
    }
}
```



- 调用函数的第**二**种方式

```js
const store = new Vuex.Store({
    state:{
        count:0
    },
    mutations:{
        add(state){
            state.count++
        },
        addN(state,step){
            state.count += step
        }
    }
})

import {mapMutations} from 'vuex'
methods:{
    ...mapMutations(['add','addN']),
    handler1(){
    	this.add();
    },
    handler2(){
        this.addN(3);
    }
}

```





> **action**

> 通过异步变更数据，必须通过`action` ，而不能使用`mutation`，但是在`action`中还是要通过触发`mutation`的方式来间接的变更数据

- 第**一**种方式

```js
const stote = new Vuex.Store({
    state:{
        count:0
    },
    mutations:{  // 不能执行异步操作，否则数据不会同步
        add(state){
            state.count++;
        }
    },
    actions:{
        addAsync(context){
            setTimeout(() => {
                context.commit('add')
            },1000)
        }
    }
    
})



methods:{
    add(){
        this.$store.dispatch('addAsync')
    }
}
```

> 第**二**种方式

```js
<div @click="addNAsync(3)">按钮</div>

import { mapActions } from 'vuex' 

methods:{
    ...mapActions(['addAsync','addNAsync'])
}
```





> :blonde_woman:  通过`action`异步传递参数和`mutation`用法差不多





> **getter**

> `getter` 用于对`store`的数据进行加工处理形成新数据
>
> - 对`store`中已有的数据加工处理，类似`vue`的计算属性
> - `store`中的发生变化，`getter`的数据也会发生变化

```js
const store = new Vuex.Store({
    state:{
        count:0
    },
    getters:{
        showNum: state => {
            return "当前最新的数量是" + state.count
        }
    }
})
```

- 第**一**种使用方式

```js
this.$store.getters.名称
```

- 第**二**种使用方式

```js
import { mapGetters } from 'vuex'

computed:{
    ...mapGetters(['showNum'])
}
```












### 使用步骤

> :one: 安装`vuex`依赖包

```markdow
npm install vuex --save
```

> :two:  导入`vuex` 依赖

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

> :three:  创建`store`对象

```js
const store = new Vuex({
    // state 中存放的就是全局共享的数据
    state : {
        count : 0
    }
})
```

> :four:  将`store`对象挂载到`vue`实例

```js
new Vue({
    el:'#app',
    render:h => h(app),
    router,
    store
})
```





# v-slot 插槽

## 匿名插槽







## 具名插槽

> 指定插槽显示内容











## 作用域插槽

> 可以进行数据绑定，父子通讯插槽







# 插件

- 懒加载 `vue-lazyload`
- 轮播 `vue-awesome-swiper`
- 异步 `vue-axios`
- Cookie  `vue-cookie`





# Storage、Cookie的区别

> 存储大小

Cookie 4K，Storage 5M

> 有效期

Cookie拥有有效期，Storage永久存储

> 存储位置

Cookie会发送到服务器端，存储在内存中，Storage只存储在浏览器端

> 路径

Cookie有路径限制，Storage值存储在域名下

> API

Cookie没有特定的API，Storage有特定的API







# Tinymce

![image-20200526141741103](D:\study data\学习笔记\Vue\Vue学习笔记.assets\image-20200526141741103.png)



> Vue中使用Tinymce的相关文章

<a href="https://www.cnblogs.com/wisewrong/p/8985471.html">在 Vue 项目中引入 tinymce 富文本编辑器</a>
<a href="https://www.cnblogs.com/zhongchao666/p/11142537.html">Vue CLI 3+tinymce 5富文本编辑器整合</a>







# 注册全局Filter的方法

创建filters.js文件

```js
const filters = {
    fmtUrl:function(url){
        return 'hello' + url
    }
}

export default (Vue) => {
    Object.keys(filters).forEach(key => {
        Vue.filter( key , filters[key])
    })
}
```



在Vue实例中注册组件

```js
import Vue from 'Vue'
import filters from './filters'

filters(Vue)
```



使用

```html
<div>
    {{ "www.baidu.com" | fmtUrl }} 
</div>
```



# 环境配置文件

[[vue-cli3.0 环境变量与模式](https://segmentfault.com/a/1190000015133974)]:"https://segmentfault.com/a/1190000015133974"