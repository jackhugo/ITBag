以下整理自[html css](http://www.w3school.com.cn/css/index.asp)

## 语法
- 当使用 RGB 百分比时，即使当值为 0 时也要写百分比符号。但是在其他的情况下就不需要这么做了。比如说，当尺寸为 0 像素时，0 之后不需要使用 px 单位，因为 0 就是 0，无论单位是什么。
~~~css
p { color: rgb(100%,0%,0%); }
~~~

- 如果值为若干单词，则要给值加引号
~~~css
p {font-family: "sans serif";}
~~~

- CSS 对大小写不敏感。不过存在一个例外：如果涉及到与 HTML 文档一起工作的话，class 和 id 名称对大小写是敏感的。

## id选择器
- id 属性只能在每个 HTML 文档中出现一次

## 背景色
- background-color 不能继承，其默认值是 transparent。transparent 有“透明”之意。也就是说，如果一个元素没有指定背景色，那么背景就是透明的，这样其祖先元素的背景才能可见。
- background-image 也不能继承。事实上，所有背景属性都不能继承。
