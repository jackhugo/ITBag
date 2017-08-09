以下整理来自[w3school jquery](http://www.w3school.com.cn/jquery/index.asp)

# 教程
## 安装
- 为什么我们没有在<script\> 标签中使用type="text/javascript" ？
在 HTML5 中，不必那样做了。JavaScript 是 HTML5 以及所有现代浏览器中的默认脚本语言。

## 语法
- $(this).hide()
演示 jQuery hide() 函数，隐藏当前的 HTML 元素
- $("#test").hide()
演示 jQuery hide() 函数，隐藏 id="test" 的元素
- $("p").hide()
演示 jQuery hide() 函数，隐藏所有 <p\> 元素
- $(".test").hide()
演示 jQuery hide() 函数，隐藏所有 class="test" 的元素

## 选择器
- 元素选择器，通过类型、id、class选择
- 属性选择器，选择带有给定属性的元素
- css选择器，用于改变css样式

# 效果
## 隐藏、显示
-  toggle() 方法来切换 hide() 和 show() 方法。
  - $(selector).toggle(speed,callback); speed可以是slow、fast、毫秒

## 动画
- 默认地，所有 HTML 元素都有一个静态位置，且无法移动。如需对位置进行操作，要记得首先把元素的 CSS position 属性设置为 relative、fixed 或 absolute！
- 当使用 animate() 时，必须使用 Camel 标记法书写所有的属性名，比如，必须使用 paddingLeft 而不是 padding-left
- 色彩动画并不包含在核心 jQuery 库中。如果需要生成颜色动画，您需要从 jQuery.com 下载 Color Animations 插件。
- 也可以定义相对值（该值相对于元素的当前值）。需要在值的前面加上 += 或 -=。ps： width:'+=150px'
- 可以把属性的动画值设置为 "show"、"hide" 或 "toggle"。ps： height:'toggle'

## 方法链接
- 链接，允许我们在相同的元素上运行多条 jQuery 命令，一条接着另一条。ps：$("#p1").slideUp(2000).slideDown(2000);

# HTML
## 获取
- 获取内容
  - text() - 设置或返回所选元素的文本内容
  - html() - 设置或返回所选元素的内容（包括 HTML 标记）
  - val() - 设置或返回表单字段的值
- 属性获取、设置，attr()
- CSS操作，css()
- 以上函数都包含回调函数，可以返回元素下标和原始值

## 添加
- append()、prepend() - 在被选元素的结尾、开头插入内容（在该元素内操作）
- after()、before() - 在被选元素之后、之前插入内容（产生新的元素）

## css类
- 添加类时，您也可以选取多个元素： $("h1,h2,p").addClass("blue");
- 您也可以在 addClass() 方法中规定多个类：$("#div1").addClass("important blue");
