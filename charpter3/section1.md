##3.1 数字

####3.1.3 JavaScript中的算术运算

    page36: JavaScript中的算术运算在溢出（overflow）、下溢（underflow）或被零整时不会报错。
    ==>结果为一个特殊的无穷大（Infinity）表示，负无穷大（-Infinity）
    0/0 没有意义，返回的结果是NaN(not-a-number)

    NaN, 它和任何值都不相等，所以无法通过NaN来判断x是否是NaN
    x!=x //判断x是否为NaN,当且仅当x为NaN的时候，结果为true


    var zero=0;
    var negz=-0;
    zero === negz   //=>true:正零值和负零值相等
    1/zero === 1/negz //=>false:正无穷大和负无穷大不相等


####3.1.4 二进制浮点数和四舍五入错误
    var x = 0.3 - 0.2
    var y = 0.2 - 0.1
    x == y     //=>false:0.3-0.2=0.09999999999999998; 0.2-0.1=0.1;
    x == 0.1   //=>false
    y == 0.1   //=>true




3.8类型的转换

"==“ 在转换前做了类型转换，比如 ””转为0

p50： x+””  // =>等价于String(x)
+x =>等价于Number(x)
!!x =>等价于Boolean(x)

// parseInt(value,radix)  //第二个参数为基数，比如二进制、八进制、16进制，默认10进制
parseInt("-12.34”)  //  -12
parseInt(".1”)         //NaN:整数不能以”.” 开始
parseFloat("$45.23”)  //NaN:数字不能以”$” 开始
parseInt(“3 blind mice”)  // 3

//对象转换成原始值
（{x:1,y:2}).toString() // =>”[object object]”

[1,2,3].toString();   // “1,2,3"
(function(x){f(x);}).toString() // "function (x){f(x);}”


function test(o){
  var i= 0;
  if(typeof o =='object'){
    var j=0;
    for(var k=0;k<10;k++){
      console.log(k);
    }
    console.log(k);    // k已经定义，输出10
  }
  console.log(j);   //j已经定义，但是可能没有初始化
}


var scope="global";
function fn(){
  console.log(scope);  // undefined , p58 ,执行 fn() ,就输出global
}

 scope="global";
function fn(){
  console.log(scope);  // global , p58
}
