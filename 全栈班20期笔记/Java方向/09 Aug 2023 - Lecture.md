# 09 Aug 2023 - Tutorial
## - 网页设计基和开发中的长度单位总结(用于指定元素的尺寸和间距)：
`px (Pixels)` -> absolute value:
像素是最基本的长度单位，表示屏幕上的一个点。它是一个固定的单位，无法随着页面缩放或设备改变而自动调整大小。

`% (Percentage)`:
百分比是相对于父元素的长度单位。例如，如果你将一个元素的宽度设置为50%，那么它将占据其父元素宽度的一半。

`vh (Viewport Height) && vw(Viewpoint Width)`:
vh 表示视窗的高度的百分比，1vh 等于视窗高度的1%。例如，如果你将一个元素的高度设置为50vh，它将占据视窗高度的50%。

`em` -> element on which it is used:
em 是一个相对长度单位，它是相对于元素的字体大小来计算的。例如，如果一个元素的字体大小为16px，那么1em 将等于16px。

`rem` -> root font size:
rem 也是一个相对长度单位，但是它是相对于根元素（通常是 <html> 元素）的字体大小来计算的。这使得 rem 在页面中的各个元素上具有一致的基准，不会受到嵌套元素的影响。

`bem (Block Element Modifier)`:
BEM 并不是长度单位，而是一种命名约定用于管理 CSS 类名。它的核心思想是将类名划分为块（Block）、元素（Element）和修饰符（Modifier）。BEM 的目的是帮助开发者编写可维护、可扩展的 CSS。
例如：
![](assets/16915749693637.jpg)
- `-`中划线仅作为连字符使用，表示某个块或者某个字元素的多单词之间的连接记号。
- `__`双下划线：用来连接块和块的子元素。
- `_`单下划线：单下划线用来描述一个块。

## Media query 
- Query syntax(媒体查询):
    媒体查询使用`@media`作为关键字，后面紧跟一个条件用于指定样式的条件。例如：
    `@media (max-width: 768px) {
    /* 在屏幕宽度小于等于768px时应用的样式 */
    }`
    
- Condition expression(条件表达式)：
媒体查询的条件表达式使用媒体特性和值来指定应用样式的条件。例如：
`width` `height` `oientation` `resolution`
`@media (min-width: 768px) and (orientation: landscape) {`
    `/* 在屏幕宽度大于等于768px且为横向方向时应用的样式 */`
`}`

- Media Type(媒体类型):
媒体查询可以指定媒体类型，比如`screen` `print` `speech`等。
- 组合媒体查询：
可以将多个媒体特性和逻辑操作符比如`and` `not` `only` 组合在一起，以更精确地定义样式应用的条件。
- Common device breakpoints
`320` `480` `600` `768` `800` `1024` `1280` `1440`
> Note: 如何想要最精确的用户体验最好的选择是用真机测评

## Position
- Default: static
- relative: -> `元素仍然占有文档流空间`
  相对定位是相对于元素自身原始位置进行定位的。使用相对定位时，元素仍然占据原始文档流中的位置，但可以通过调整top、right、bottom和left属性来移动元素。相对定位不会影响其他元素的布局。
- absolute: -> `元素脱离了文档流`
  绝对定位是相对于最近的已定位祖先元素或父元素进行定位的。如果没有已定位的祖先元素，元素会相对于文档的初始包含块进行定位。绝对定位的元素脱离了文档流，不再占据空间，因此其他元素可能会占据它原来的位置。
- fixed: -> `元素脱离了文档流`
  固定定位是相对于视口（浏览器窗口）进行定位的，无论页面滚动与否，固定定位的元素始终保持在相同的位置。固定定位的元素也脱离了文档流。
- sticky: -> 元素会在指定的偏移阈值内保持固定，超过阈值后会恢复正常文档流。
  粘性定位可以让元素在滚动过程中表现得像相对定位和固定定位的结合体。相对父元素进行定位，`top` `left` `right` `bottom` 等偏移量也是相对于父元素的边界， 必须添加偏移量的其中一个才会生效。  
> Note: 在使用这些定位方式的时候，通常需要结合使用`top` `left` `right` `bottom`等属性来调整元素的位置。不同的定位方式适用于不同的布局需求。

## Transition
![](assets/16915787668969.jpg)

- z-index: 数值越大，该元素越在层级最上面显示，默认为`0`
> Note: 仅对带有position属性的box生效。


# Sass(Syntactically Awesome StyleSheets)
- Sass是css的一种预处理语言，css的预处理语言都有`less` `sass` `scss`
- 这些预处理语言只有在开发阶段使用。
- html是无法解析预处理语言的，会在运行的时候编译为css
- 为什么要使用scss
    - Scss完全兼容所有版本的CSS。
    - 特性丰富，Scss拥有比其他任何CSS扩展语言更多的功能和特性。
    - 技术成熟，功能强大。
    - 行业认可，越来越多的人使用Scss。
    - 社区庞大，大多数科技企业和成百上千名开发者为Sass提供支持。
    - 有无数框架使用Scss构建，如Compass，BootstrapB，ourbon和Susy。
- SASS has 5 primary features(核心特性就是复用)
    - Variables
        可以用属性引用（复用）
        ```
        $linkColor: #08c;
        a {
            color: $linkColor;
        }
        h1 {
            color: $linkColor;
        }
        .nav {
            color: $linkColor;
        }
        ```
    - Nesting（嵌套）
    - Mixin（代码的复用和传参）
        ```
        @mixin style($size: 15px,$color:#999) { 
            font-size: $size; 
            color:$color; 
            line-height: 1.5;
            content {
                @include style(20px,black); 
                margin:20px;
            header {
                @include style(25px,blue); 
            footer {
                @include style(30px,red);
            }
        ```
    - Partials
    - Extent/Inheritance（`%`不是必须的，可以直接继承某一个选择器的样式）可以继承另一个选择器的所有样式
        ```
        %ir{
            text-shadow: none;
            background-color: blue;
            border: 0;
        }
        
        #header{
            @extend %ir;
        }
        #footer{
            @extend %ir;
        }
        ```
- Function
    ```
     @function calculate($width, $height){
        @return(
            color: $width * $height;
        );
     }
     
     $area: calculate(10px, 20px);
    ```        
        