# JSTL核心标签库

``` html
<c:if></c:if>//标签
```

1. 语法格式：

```html
<c:if test = "" [var = "xxx"][scope = "{page|request|session|application}"]>
	执行体
</c:if>
```

2. 标签属性：

- test：用于设置逻辑表达式，当逻辑表达式为true时执行执行体
- var：用于指定逻辑表达式中变量的名字
- scope：用于指定var变量的作用范围，默认值是page。

![image-20201008172029238](/Users/yangxiansheng/笔记/images/Mybatis缓存.png)

2. c:forEach标签

两种语法格式：

1. 迭代集合对象：

```html
<c:forEach item = "xxx" [var = "xxx"] [varStatus = "xxx"] [begin = "xxx"] [end = "xxx"] [step = "xxx"]>
	循环体
</c:forEach>
```

1. 迭代数组

```html
<c:forEach item = "xxx" [var = "xxx"] [varStatus = "xxx"]  [step = "xxx"]>
	循环体
</c:forEach>
```

标签属性：

- items：用于指定将要迭代的对象
- var：用于指定当前迭代状态信息的对象保存到page域中的名称（可以通过EL表达式${xxx}取得对象）
- varStatus：用于指定当前迭代状态信息的对象保存到page域中的名称

> varStatus可以获取以下信息：
>
> - count：表示元素在集合中的序号，从1开始
> - index：表示当前元素在集合中的索引，从0开始
> - first：表示当前是否为集合中的第一个元素
> - last：表示当前元素是否为集合中最后一个元素

- begin：用于指定从集合中的第几个元素开始迭代，begin的索引值从0开始
- step：用于指定迭代的步长，即迭代的增长因子