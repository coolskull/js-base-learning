1.1 JavaScript语言核心


    "2"+"3" // =>"32": "+"可以完成加法运算也可以作字符串连接
    "two" > "three" // =>true: "tw"在字母表中的索引大于"th"


 将函数和对象合写在一起时，函数就变成了“方法”（method）

    var points = [{
        x: 0,
        y: 0
    }, {
        x: 1,
        y: 1
    }];

    points.dist = function() {
        var p1 = this[0];
        var p2 = this[1];
        var a = p2.x - p1.x;
        var b = p2.y - p1.y;
        return Math.sqrt(a * a + b * b);
    }

    points.dist();
