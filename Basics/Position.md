#  绝对定位与相对定位
## 浮动
典型的问题就是一个父元素包含了多个浮动的子元素。页面的内容设置了一个宽度，子元素的浮动确定了他们的位置，但浮动元素不会影响父元素的宽度。这样做会让父元素塌陷，从而使父元素的高度为“0”，以及忽略其他的属性。“clearfix”技巧是基于在父元素上使用“:before”和“:after”两个伪类。在IE6和7的浏览器中，加上“*zoom”属性来触发父元素的hasLayout的机制。决定了元素怎样渲染内容，以及元素与元素之间的相互影响。
## static
这个关键字使得这个元素使用正常的表现，即元素处在文档流中它当前的布局位置，top, right, bottom, left 和 z-index 属性无效。
## Position absolute
脱离了文本流（即在文档中已经不占据位置），参照浏览器的左上角通过top,right,bottom,left（简称TRBL） 定位。 absolute在没有设定TRBL值时是根据父级对象的坐标作为始点的，当设定TRBL值后则根据浏览器的左上角作为原始点。不为元素预留空间，元素位置通过指定其与它最近的非static定位的祖先元素的偏移来确定。绝对定位的元素可以设置外边距（margins），并且不会与其他边距合并。

## Postion relative
相对于元素本身在文档中应该出现的位置来移动这个元素，可以通过TRBL来移动元素的位置，实际上该元素依然占据文档中原有的位置，只是视觉上相对原来的位置有移动。会适应该元素的位置，并不改变布局（这样会在此元素原本所在的位置留下空白）。position:relative对table-*-group, table-row, table-column, table-cell, table-caption无效。不为元素预留空间，元素位置通过指定其与它最近的非static定位的祖先元素的偏移来确定。绝对定位的元素可以设置外边距（margins），并且不会与其他边距合并。
## fiixed
不为元素预留空间。通过指定相对于屏幕视窗的位置来指定元素的空间，并且该元素的位置在屏幕滚动时不会发生改变。打印时元素会出现在的每页的固定位置。fixed属性通常会创建新的栈环境。


## 居中问题
 各种不同形式的居中。   https://css-tricks.com/centering-css-complete-guide/

###### 水平居中
text-align:center 和 margin:0 auto  
前者是针对父元素进行设置而后者则是对子元素。他们起作用的首要条件是子元素必须没有被float影响，否则一切都是无用功。
###### 垂直居中
没有margin:auto 0 的用法。可以参考上述链接中的方法。

设置Position居中：首先给父元素写上positon:relative，这么做是为了给子元素打上position:absolute的时候不会被定位到外太空去。接下去，写上子元素的height和width，这个似乎是必须的
，某些浏览器在解析的时候如果没有这2个值的话会出现意想不到的错位。接着就是整个方法的核心，给子元素再打上top:50%;left:50%以及margin-top:一半的height值的的负数;
margin- left:一半的weight值的负数。整理一下之后，可能你会给你的子元素写上这样的css（当然，父元素也要先写上width和height）
```{width:100px;height:80px;position:absolute;top:50%;left:50%;margin-left:50px;margin-top:40px}```
