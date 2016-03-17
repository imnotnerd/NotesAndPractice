# Less
 ### Variable
变量是“按需加载”（lazy loaded）的，因此不必强制在使用之前声明。也可以在定义变量值时使用其它的变量：
<code><pre>@fnord: "I am fnord.";
@var: 'fnord';
content: @@var;</pre></code>
上述代码解析后content内容为 i am fnord.   
### Mixins
在 LESS 中我们可以定义一些通用的属性集为一个 class，然后在另一个 class 中去调用这些属性。

@arguments包含了所有传递进来的参数。 如果你不想单独处理每一个参数的话就可以像这样写：<code><pre> .box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
    box-shadow: @arguments;
    -moz-box-shadow: @arguments;
    -webkit-box-shadow: @arguments;
}
.box-shadow(2px, 5px);</pre></code>
如果需要在 mixin 中不限制参数的数量，可以在变量名后添加 ...，表示这里可以使用 N 个参数。.mixin (@a, @rest...) 表示@a后的所有参数。
### 嵌套规则
``` #header { color: black;
    .navigation { font-size: 12px }
    .logo { width: 300px;
        &:hover { text-decoration: none }
    }
} ```

如果你想写串联选择器，而不是写后代选择器，就可以用到 & 了。这点对伪类尤其有用如 :hover 和 :focus。
#### 嵌套Media queries
``` .one {
    @media (width: 400px) {
        font-size: 1.2em;
        @media print and color {
            color: blue;
        }
    }
}
输出：
@media (width: 400px) {
    .one {
        font-size: 1.2em;
    }
}
@media (width: 400px) and print and color {
    .one {
        color: blue;
        }
}```
#### &的用法
用在选择器中的&还可以反转嵌套的顺序并且可以应用到多个类名上。
