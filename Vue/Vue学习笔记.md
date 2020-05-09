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





# p15

