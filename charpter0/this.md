##关于this
###为什么要用this
`this`提供一种更优雅的方式来隐式"传递"一个对象引用，因此可以将API设计得更简洁。
```js
function identify() {
    return this.name.toUpperCase();
}

function speak() {
    var greeting = "Hello, I'm " + identify.call( this );
    console.log( greeting );
}

var me = {
    name: "Kyle"
};

var you = {
    name: "Reader"
};

identify.call( me ); // KYLE
identify.call( you ); // READER

speak.call( me ); // Hello, I'm KYLE
speak.call( you ); // Hello, I'm READER
```

如果不使用`this` `↓`

```js
function identify(context) {
    return context.name.toUpperCase();
}

function speak(context) {
    var greeting = "Hello, I'm " + identify( context );
    console.log( greeting );
}

identify( you ); // READER
speak( me ); // Hello, I'm KYLE
```
###误解

#####误解1：this指向本身
`this`并不是向我们所想的那样指向函数本身
```js

function foo(num) {
    console.log( "foo: " + num );

    // keep track of how many times `foo` is called
    this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
    if (i > 5) {
        foo( i );
    }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// how many times was `foo` called?
console.log( foo.count ); // 0 -- WTF?
```
可以用浏览器调试工具console.log`foo` 函数里面的`this`,就会发现`this`指向的是`window`

#####call方法
还有另外一种方法更关注`this`在`foo`函数对象中实际指向的问题。
```js
function foo(num) {
    console.log( "foo: " + num );

    // keep track of how many times `foo` is called
    // Note: `this` IS actually `foo` now, based on
    // how `foo` is called (see below)
    this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
    if (i > 5) {
        // 使用 `call(..)`, 可以全包this指向函数对象'foo'本身
        foo.call( foo, i );
    }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// how many times was `foo` called?
console.log( foo.count ); // 4
```
强制this指向foo函数对象。

#####误解2：this指向函数的作用域

```js
function foo() {
    var a = 2;
    this.bar();
}

function bar() {
    console.log( this.a );
}

foo(); //referenceError:a is not defined??? //为什么浏览器调试为2
```

这段代码中的错误不只一处:   
1.试图通过`this.bar( )`来引用`bar`函数。这样调用能主要是浏览器调试中this指向window??? 
2.试图使用`this`联通`foo( )`和`bar( )`的词法作用域。



##### 参考材料：
 《你不知道的JavaScript上卷》    
 [深入浅出妙用 Javascript 中 apply、call、bind](http://web.jobbole.com/83642/)
