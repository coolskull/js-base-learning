##2.5 可选的分号

    var y=x+f
    (a+b).toString()
    =>解析成  var y=x+f(a+b).toString( );


    return
    true;
    =>解析成 return； true；
      所以人return、break、continue和随后的表达式不能随意的换行。


    x
    ++
    y
    =>解析成 x; ++y;
