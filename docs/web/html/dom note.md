以下整理自[html dom](http://www.w3school.com.cn/js/js_htmldom.asp)

## 删除元素
两种方法

~~~javascript
var parent=document.getElementById("div1");
var child=document.getElementById("p1");
parent.removeChild(child);

var child=document.getElementById("p1");
child.parentNode.removeChild(child);
~~~

## navigator
- 来自 navigator 对象的信息具有误导性，不应该被用于检测浏览器版本，这是因为：1、navigator 数据可被浏览器使用者更改；2、浏览器无法报告晚于浏览器发布的新操作系统
