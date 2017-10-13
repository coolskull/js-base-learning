##原型
```js
var anotherObject={
    a:2
}
var myObject=Object.create(anotherObject);

anotherObject.a; //2
myObject.a; //2

anotherObject.hasOwnProperty("a"); //true
myObject.hasOwnProperty("a"); //false

myObject.a++; 
//相当于myObject.a=myObject.a+1;
// 因此++操作会通过[[prototype]]查找属性a并从 anotherObject.a获取当前属性值2，
// 然后给这个值加去，接着用[[put]]将值2赋给myObject中新建的屏蔽属性a

anotherObject.a; //2
myObject.a; //3

myObject.hasOwnProperty("a"); //true

```
p157-p
