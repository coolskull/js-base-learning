##3.8 类型的转换

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
