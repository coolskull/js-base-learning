###对象
###3.2类型
注意：简单基本类型（string，boolean,number,null和undefined）本身并不是对象。
null有时会被当作一种对象类型，但是这其实只是语言本身的一个bug，即对null执行typeof null时会返回字符串"object"。实际上，null本身是基本类型。


###3.3.4 复制对象

粗糙的复制：使用json，要求json安全
```js
var newObj=JSON.parse(JSON.stringify(someObj))
```
ES6定义了Object.assign(..)实现浅复制

#####禁止扩展
Object.preventExtension(..)
#####密封
Object.seal(..)
#####冻结
Object.freeze(..)

###3.3.10 存在性
```js
var myObject = {
    a: 2
};
("a"in myObject); //true,in 会检查属性是否在对象及其[[prototype]]原型链中
("b"in myObject);//false
myObject.hasOwnProperty("a");//true,hasOwnProperty只会检查是否在对象中
myObject.hasOwnProperty("b");//false
```
for..in不能访问不可枚举的属性，一般用在对象上。
用在数组上会产出出人意料的结果，因为种种枚举不仅会包含所有数值索引，还会包含所有可枚举属性。
