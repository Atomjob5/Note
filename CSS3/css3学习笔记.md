# 选择器

> 后代选择器 `> ` 只影响孩子

```html
<body>
    <div> ←只作用这个
        <div></div>
    </div>
</body>

<style>
    body > div{
        border:1px solid red;
    }
</style>
    

```



> 兄弟选择器 `~`   和  `+`

- `~` 作用元素后全部兄弟
- `+` 作用元素紧邻的兄弟



> 属性选择器 `[key="value"]`

- `^`  开始
- `$`  结束

- `*`  任意地方
- `~`  词匹配
- `|`  开始或用`-`连接的词



>  伪类选择器

- `hover`
- `focus`
- `visited`
- `active`
- `link`
- `root`
- `empty`
- `first-child`  后代的第一个元素，会遍历，加 `>` 可用来限制
- `only-child`  唯一
- `only-of-type` 
- `nth-of-child()`   使用 `n` 代表选择全部 ，`2n`偶数，`2n-1` `odd`奇数，`even`全部，`-n+2`前两个，`n+2`第二个开始
- `nth-of-type()`
- `nth-last-child()`
- `nth-last-of-type()`
- `not()`

```css
main > ul li:nth-child(n):not(:nth-child(1))
```



# 文字

> `letter-spacing`  文字间距



> `text-transform`  文字形式







# 栅格系统

> 基础页面

```css
div{
    width:300px;
    height:300px;
    border:1px solid blue;
    display:grid;  #声明栅格系统
}
```

> 使用`px`

```css
grid-template-rows:100px 100px 100px;
grid-template-cols:100px 100px 100px;
```

> 使用`repeat`

```css
grid-template-rows:repeat(3,100px);
grid-template-cols:repeat(3,100px);
```

> 使用`%`

```css
grid-template-rows:50% 50%
grid-template-cols:20% 20% 20% 20% 20%
```

> 使用`fr`

```css
grid-template-rows:1fr 2fr 1fr;
grid-template-cols:1fr 1fr;
```

> 同时使用多个`px`

```css
grid-template-rows:repeat(2,100px 50px);
grid-template-cols:repeat(2,50px);
```

> 自动填充 `auto-fill`

```css
grid-template-rows:repeat(auto-fill,100px);
grid-template-cols:repeat(auto-fill,100px);
```

> 尺寸波动范围`minmax()`

```css
grid-template-rows:repeat(3,minmax(50px,100px))
```

> 控制间距 `row-gap` 和 `column-gap`

```css
row-gap:10px;
column-gap:10px;
```

> 根据栅格线放置元素

```css
grid-row-start:1;
gird-row-end:2;
gird-column-start:1;
gird-column-end:3;
```

> 自定义栅格命名

```css
grid-template-rows:[r1-start]100px [r1-end r2-start]100px [r2-end r3-start]100px[r3-end];
```

> 根据偏移量定位元素

```css
grid-column-end: span 3;
gird-column-end: span 2;
```

> 元素定位简写 `gird-row` 和 `grid-column`

```css
grid-row: 1/2;
grid-column: 1/4;
```

> 也可以使用偏移 `span`

```css
grid-row: 1/span 2;
grid-row: 1/span 4;
```

> 使用区域`grid-area`部署元素

```css
grid-area: 1 / 1 / 4 / 4;
```

> 使用区域定位部署元素

```html
<html>
    <div>
        <header></header>
		<nav></nav>
        <main></main>
        <footer></footer>
    </div>
</html>

<style lang="css">
    div{
        /* 划分元素 */
        grid-template-rows:60px 1fr 60px;
        grid-template-cols:60px 1fr;
        grid-template-areas: "header header" "nav main" "footer footer"
    }
    
    header:{
        grid-area:header;
    }
    
    nav:{
        grid-area:nav;
    }
    
    main:{
        grid-area:main;
    }
    
    footer:{
        grid-area:footer;
    }

</style>

```

> 栅格区域命名占位符

```css
grid-template-areas:"header header"
					". ."
					"footer footer"
```

> 栅格流动方向 `grid-auto-flow`

```css
/* 行方向 */
grid-auto-flow: row;
/* 列方向 */
grid-auto-flow: column;
```

> 栅格流动填满空隙 `dense`

```css
grid-auto-flow: row dense;
```

> 栅格整体对齐方式处理

```css
justify-content: [left / center / right / space around
				/ space between / space evenly];
align-centent: center;
```

```css
justify-items: [start / end / center / stretch];
align-item: [start / end / center / stretch];
```

```css
justify-self: start / end / center
align-self: start / end / center
```

> 栅格对齐的简写

```css
place-content: center center;
```

```css
place-items: center center;
```

```css
place-self: center center;
```







# 定位布局

> 相对定位 `relative`

- 不会脱离文档流，仍然**保留**原来位置
- 子元素要使用绝对定位时，父元素可使用`relative`控制子元素位置

> 绝对定位 `absolute`

- 脱离文档流，**不保留**原来位置

> 通过定位设置尺寸

如果子元素不设置宽高，可以直接通过子元素的`absolute`来定位边界

```html
<html>
    <body>
        <div class="parent">
            <div></div>
        </div>
    </body>
    
    <style>
        .parent{
            width:300px;
            height:300px;
            margin:0 auto;
            position:relative;
            overflow:hidden;
        }
        
        .parent:frist-child{
            position:absolute;
            /* 不指定宽度和高度 */
            left:250px;
            right:50px;
            top:100px;
            bottom:100px;
        }
    </style>
</html>
```



> 控制元素**居中**定位的几种方式

- 直接计算
- 通过偏移自身大小

```css
position:absolute;
width:200px;
height:200px;
left:50%;
top:50%;
margin-left:-100px;
margin-top:-100px;
```



> 多级定位问题

寻找父级定位为`relative`的最近的元素



> 滚动对定位的影响

元素会随着页面滚动而移动





# 盒子模型

> 固定为盒子大小不受内边距影响

```css
box-sizing: border-box;
```

> `overflow`  文本溢出



> `white-space`  



> `white-pace`



> 尺寸控制

```css
max-width:100px;
max-height:100px;
min-width:50px;
min-height:50px
```



> `fill-avaliable`  自动撑满整个空间



> `fit-content`  自动适应内容长度





# 弹性盒子

> `flex-direction`  主轴



> `flex-wrap`  换行



> `flex-flow`  [ 主轴方向 ]  [ 换行方式 ] 



> `stretch` 属性的注意点

设置任意`width`  `height`  `max-width`  等都会让 `stretch` 失效，优先度都比`stretch`高



> 主轴的属性

- `justify-content`  



> 交叉轴的属性

- `align-items` 

- `align-content`



> 元素可用空间分配 `flex-grow`



> 控制元素缩小比例`flex-shrink`



> 主轴基础尺寸 `flex-basic`

**优先级：** `max-width`  >  `flex-basic`  >  `width` 



> 简写  `flex`

```css
flex: [flex-grow] [flex-shrink] [flex-basic]
```



> 控制弹性元素的排序 `order`







# 颜色渐变

> `background-image`   

```css
background-image: url(xxx.jpg)
```

> `background-clip`  背景裁切

- `content-box`
- `border-box`
- `padding-box`

> `background-repeat` 背景重复



> `background-attachment`  背景固定



> `background-position` 



> `background-size`



> 盒子阴影 `box-shadow`

```css
box-shadow: [x] [y] [羽化度] [rgba];
```



> 线性渐变  `linear-gradient`

```css
background: linear-gradient( [deg] , [r] , [g] , [b] );
```



> 镜像渐变 `radial-gradient`



> 镜像标志位 

```css
background: linear-gradient(45deg, red 50%, yellow 50%);
background: radial-gradient(red , yellow 50%, black 100%)
```



> 渐变中间阈值

```css
background: linear-gradient(90deg,red,50%,green);
```



> 重复渐变

```css
background: repeating-linear-gradient(45deg,blue,25px,yellow 25px,25px,red 50px);
```



# 过渡效果

> `transition-property`  过度效果

- `background`  背景颜色过渡
- `width`  宽度过渡
- `height`  高度过渡

- `all`  所有

> `transition-duration`  过渡时间



> 动画与`js`的API接口 `transitionend`

```js
document.querySelector('div').addEventListener('transitionend',function(e)){
	document.querySelector('div').className = 'move';
}
```



> `transition-timing-function`  运行轨迹



> `transition-delay`  延迟过渡





> 简写

```css
/* 全部 */
transition: [transition-property] [transition-timing-fuction] [transition-duration] [transition-delay] 

/* 单独 */
transition: background ease 1s 1s,border-radis linear 1s 0s;
```



# 变形与透视

> `translate`

```css
/* 根据元素自身尺寸的百分比，如果元素为100px宽度，则移动100/2=50px */
transition: translate(50%, 100px)  
```



> `perspective`  透视

透视时会计算子元素



> `perspective-origin`  观看角度



> `rotate`  旋转



> `scale`  缩放



> `transform-origin`  缩放基线



> `transform-style`  



> `filter`  高斯模糊



> `skew`  倾斜

# 动画

> `@keyframes [名称] `  

```css
@keyframes kf{
    from{
        
    }
    to{
        
    }
}
```



> `animation-name`



> `animation-duration`



> `animation-delay`  动画延迟



> `animation-fill-mode`  填充模式



> `animation-iteration-count`   动画循环次数

- `infinite`  无限循环



> `animation-direction`  动画方向



> `animation-delay`  延迟



> `animation-timing-function`

- `steps( [step] , [start / end] )`  步进



> `animation-play-state`  动画状态



> `简写`

```css
animaiton: [name] [duration] [delay] [fill-mode] 
```

