# 知识点
1. Nth-child() 表示父元素的第几个孩子，和标签类型无关。可以用nth-of-type() 来选择第n个某个类型的标签。 比如：ul li:nth-of-type(2) 选择ul下面第二个li标签
2. Em文字根据父元素的大小计算，width 和height是根据当前大小计算来的
3. Rem 根据根元素来计算
4. Bem命名block__element--modifier
5. Media @media(max-width : 600px) 表示小于等于 600 px, 用于响应式布局
6. 定位 position relative ， 是根据原来位置移动，相对定位， 原来的位置还是被占领，其他盒子无法占有。
7. Position absolute 相对于父元素移动，没有父元素，就根据document定位。祖先有定位，会根据最近的祖先定位。原来的位置会被别的盒子占领。 必须设置top，left ，bottom ，right 其中一个
8. Position fixed 跟父元素无关系，不随滚动条移动，不占领原来位置,  参照可视窗口
9. Position sticky  根据可视窗口移动元素
10. Box-shadow: h-offset v-offset blur spread color 设置盒子阴影
11. Z-index 要设置position。 Z-index 用于提高层级，显示在前面
12. 文本溢出显示省略号，overflow:hidden ; text-overflow: ellipsis; 要配合white-space：no wrap
13. Sass: Variables $变量名：值 ， 更改时比较方便。
14. Sass: Nesting， 书写css更方便，可读性强。
15. Sass: Mixin 定义样式， 定义：@mixin style(可传参数设置变量$变量){样式}，引用： @include style(可传参数) . 可以复用
16. Sass：extend 继承 定义:%名字{样式}  继承： @extend%名字
17. Sass：函数 @function 函数名(){}