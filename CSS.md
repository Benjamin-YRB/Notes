# CSS

### 定位(Positioning)

​	定位允许你对元素进行定位

#### 一切皆为框

​	div、h1、p等块级元素显示一块内容，即“块框”，行内元素显示为“行内框”

​	可以使用display属性改变生成的框的类型。

```css
//改变元素的框类型
display: block //块框
display: inline //行内框
display: inline-block //
display: none //没有框，即元素不显示，也不占用空间
```

> - inline（行内元素）
>   - 使元素变为行内元素，拥有行内元素的特性，即可以和其他行内元素共享一行，不会独占一行
>   - 不能更改元素的高宽，大小由内容撑开
>   - 可以使用padding，使用margin只有左右有效
> - block（块级元素）
>   - 使元素编程块级元素，独占一行，再不设置自己宽度的情况下，块级元素会默认填满父元素的宽度
>   - 能够改变元素的高宽
>   - 可以使用padding 和margin
> - inline-block（融合行内与块级）
>   - 不独占一行的块级元素

### css定位机制

​	三种定位机制：普通流、浮动和绝对定位

​	默认使用普通流定位，即元素的默认位置由元素在HTML中的位置决定

​	块级框从上到下一个接一个的排列，框之间的垂直距离由垂直外边距计算得出。

​	行内框在一行中水平布置。可以使用水平内边距、边框和外边距调整它们之间的间距。但是垂直内边距、边框和外边距不影响行内框的高度。由一行形成的水平框成为行框（line box），行框的高度总是足以容纳它所包含的所有行内框。不过设置行高可以增加这个框的高度	

#### position属性

属性值：

- static 元素框正常生成。块级元素生成一个矩形框，作为文档流的一部分，行内元素则会创建一个或多个行框，置于其父元素中。
- relative 元素框偏移某个距离。元素仍保持其未定位前的形状，他原本所占的空间仍保留
- absolute 元素框从文档流完全删除，并相对于其包含块定位