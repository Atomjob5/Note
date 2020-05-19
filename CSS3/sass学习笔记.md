<h1>Sass学习笔记</h1>

# 编译

## 手动编译

```markdown
sass sass/[origin file]/[target file]
```





## 自动编译

```markdown
sass --watch [origin src]:[target src]
```





## 输出格式

```
sass --watch [origin src]:[target src] --style [输出格式]
```

- `netsted` 嵌套

- `compact` 紧凑
- `expanded` 扩展   容易阅读
- `compressed` 压缩   体积最小





# 变量

```scss
$[变量名] : [变量值];
```



# 嵌套



### 嵌套调用父选择器

```scss
&:hover

& &-text
```



### 嵌套属性

```scss
// eg.
font:{
    family: YaHei;
    size: 15px;
    weight: normal;
}

border: 1px solid red {
    left: 0;
    right: 0;
}
```



# `mixin`

```scss
// 声明
@mixin 名字 (参数1，参数2 ...){
    ...
}

// 调用
.foo{
    @include [名字]
}
```



# 继承和扩展

```scss
.alert{
    padding: 15px;
}

.alert-info{
    @extend .alert;
    background-color: snow;
}
```



# `@import`

```scss
@import ["file"]
```





# 注释

```scss
/*
	多行注释，会包含在没有压缩之后的CSS里面
*/

// 单行注释不会出现在CSS里面

/*! 强制输出的注释内容 */
```

