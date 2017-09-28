##3.6 包装对象

    var s = "test";
    s.len = 4;
    var a = s.len;    // =>a为undefined


    var o = { x: 1 }, p = { x: 1 };
    o === p;   // =>false:两个单独的对象永不相等


    var a= [ ], b= [ ];
    a===b; // =>false:两个单独的数组永不相等
