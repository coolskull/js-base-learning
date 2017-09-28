##4.8运算符
####page70：
    1+{}  // "1[object object]"
    true+true //2
    2+null   //2
    2+undefined //NaN
    1+2+"blind mice" //"3 blind mice"
    1+(2+"blind mice" ) //"12 blind mice"

####page72:
    var i=1; j=i++; //i=2,j=1
    var i=1;j=++i；// i=2,j=2

    x++并不完全等于x+1
    如果x为”1”, x++会转为数字计算，为2，x+1会转为字符计算为 “11”

####4.8.3 位运算

位运算符会将NaN、Infinity和-Infinity都转换为0
