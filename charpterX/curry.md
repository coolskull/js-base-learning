

#####curry的概念很简单：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。


```js
var add=function(x) {
  return function(y) {
    return x+y;
  }
}
var increment=add(1);
var addTen=add(10);
increment(2);  // 3
addTen(2);  // 12

```


####参考资料
[不仅仅是双关语咖喱](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch4.html#不仅仅是双关语咖喱)
[函数式编程入门教程](http://www.ruanyifeng.com/blog/2017/02/fp-tutorial.html)
