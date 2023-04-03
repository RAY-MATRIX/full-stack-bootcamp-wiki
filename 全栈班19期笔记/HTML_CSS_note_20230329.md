# 知识点：
 1. 文件名和comment用英文
 2. 标签选择器---标签名，类选择器---.+类名，ID选择器---#+ID
 3. class 可以被复用，ID 是有唯一性的，在一个页面只能用一次
 4. 打多个标签，标签名*n，n代表个数，比如div*10 代表打10个div
 5. 标签和类名一起打出来，标签名.类名  标签和ID一起打出来，标签名#ID
 6. 标签嵌套 标签1 > 标签2 比如 ul>li
 7. 5 个 div class = demo1 demo2...demo5, 用 .demo$*5
 8. 给标签添加内容 标签{内容} 例子：div{hello world}====> <div> hello world</div>
 9. 5个div 内容1，2，3，4，5   div{$}*5s
 10. 公共样式可以提出来，写在一个class里面，可供其他标签使用，class 可以复用。一个标签里可含多个class
 11. ID只能出现一次， ID优先级高，ID>class。ID用的比较少， 在CSS优先级上会出一些问题。
 12. 通配符 *， *{}选择所有元素，可以设置html,body,div,span,li 等等标签， 优先级低
 12. text 属性 ： color  字体颜色， text-align， text-indent缩进， line-hight，text- decoration
13. line-hight 包括上间距+下间距+文字高度
14. 文字居中 用 text-align：center，除去下划线：text- decoration：none
15.  font 复合属性，可以将属性合写，font size和font Family，是必写的
16. CSS引入三种模式，1.inline 行内样式 只控制一个标签，2.内部样式只控制一个页面，3.外部样式，控制多个页面，有CSS格式的文件
17. 标签1+空格+标签2，选择标签1下面的所有标签2，包含所有后代。
18. 标签1+>+标签2  只选择标签1下面的子元素，只选择儿子，不选孙子等。
19. a标签的伪类     a：link 未访问链接  a：visited 点击过的链接 a：hover 鼠标经过的链接   a：active 鼠标正在按下没有弹起的链接
20. Focus伪类 input：focus，聚焦到输入框 
21. 行高和盒子的高度设置成一样，文字可以垂直居中
22. a标签里面不可以再放链接
23. flex ： justify-content控制主轴，align-items 控制副轴
24. background-repeat 背景图片默认平铺，用background-repeat ：no-repeat 不平铺
25. flex 主轴方向默认值 水平方向从左到右 flex-direction:row，反向排列 flex-direction: row-reverse, 水平方向从右到左，元素也会从右到左排列
26. flex-direction: column ,主轴顺时针旋转90度，方向从上到下
27. 主轴换行 flex-wrap, 默认nowrap，默认不会换行，元素超过盒子长度，会压缩元素。 flex-wrap:wrap, 超过长度，元素会换行。 
28. 主轴对齐方式， justify-content 属性有 flex-start主轴起始点对齐，默认最左边。flex-end 默认最右边对齐。 center 居中对齐。space-around 每个元素之间的距离是元素距边缘距离的两倍。 space-between 每个元素之间的距离相等，元素距边缘距离为零。 space-evenly 每个元素之间的距离和·元素距边缘距离相等。
29. 侧轴对齐，aligne-content 比 justify-content 多一个属性，stretch，如果不设置高度，aligne-content：stretch 会将元素侧轴方向拉伸，充满侧轴方向。
30. 利用flex, 可使元素水平居中，justify-content：center，align-items: center.     注意: align-items用于单行元素， align-content 用于多行元素。


 