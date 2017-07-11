以下整理来自[w3school javascript](http://www.w3school.com.cn/js/index.asp)

## js 变量
- 变量必须以字母开头
- 变量也能以 $ 和 _ 符号开头（不过我们不推荐这么做）
- 变量名称对大小写敏感（y 和 Y 是不同的变量）
- 提示：JavaScript 语句和 JavaScript 变量都对大小写敏感。
- 如果重新声明 JavaScript 变量，该变量的值不会丢失
~~~javascript
var carname="Volvo";
var carname;
~~~

## js 数据类型
- 字符串、数字、布尔、数组、对象、Null、Undefined
- JavaScript 拥有动态类型。这意味着相同的变量可用作不同的类型
- Undefined 这个值表示变量不含有值。
- 可以通过将变量的值设置为 null 来清空变量。

## js 对象
- 对象由花括号分隔。在括号内部，对象的属性以名称和值对的形式 (name : value) 来定义。
- 对象创建有多种方式，并且还可以向已存在的对象添加属性和方法。
~~~javascript
var person={firstname:"Bill", lastname:"Gates", id:5566};
var name = person.lastname;

person=new Object();
person.firstname="Bill";
person.lastname="Gates";
person.age=56;
person.eyecolor="blue";
~~~
