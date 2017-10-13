####数组
javascript数组是无类型：数组元素可以是任意类型
```js
var count=[1,,3]; //数组有三个元素，中间的那个元素值为undefined
var undefs=[,,];  //数组有两个元素，都是undefined

a[-1.23] = true;  //这将创建一个名为“-1.23”的属性
a["1000"] = 0;    //这是数组的第1000个元素
a[1.00]           //和a[1]相等

```
join:如果不指定分隔符，默认使用逗号。
shift/unshift的操作和push/pop的方向相反
reduce/reduceRight可以用来累加计算

#####数组类型
```js
Array.isArray([])    //=>true
Array.isArray({})    //=>false
[] instanceof Array  //=>true
({}) instanceof Array//=>false
```
