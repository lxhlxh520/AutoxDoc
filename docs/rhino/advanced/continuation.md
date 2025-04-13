# 协程

## Promise.coroutine()

**v6.3.7 新增**
在该版本引入 bluebird 后你可以使用 bluebird 带来的一项协程特性，这个方法接收一个**Generator 函数**作为参数，函数内部可以像**async 函数**一样编写，返回一个返回值为`Promise`的函数，说起来可能有点难以理解，具体看下方例子

```js
//async函数写法
let main = async function (s) {
  var result = await Promise.resolve("value:" + s);
  return result;
};
//Generator 函数写法
let main = Promise.coroutine(function* (s) {
  var result = yield Promise.resolve("value:" + s);
  return result;
});

main("test").then(log);
```

可以看到只要将 await 关键换成 yield 就能够代替还不支持的 async 函数，下方是一个异步循环的例子

```js
let main = Promise.coroutine(function* (size) {
  for (var i = 0; i < size; i++) {
    yield Promise.delay(1000);
    log(i);
  }
  log("end");
});
main(10); //在控制台每秒输出一个数字
```

与 async 函数不同的是 yield 关键字作为表达式使用时必须带上圆括号，否则会有语法错误，在默认情况下 yield 后面只能是 Promise 或带有 then 方法的对象，要处理其他类型数据需要使用`Promise.coroutine.addYieldHandler()`添加处理器

## Promise.coroutine.addYieldHandler(handler)

- handler `{function}` 处理函数

函数的参数为 yield 的导出，反回值为 yield 的反回

## bluebird-co

[bluebird-co](https://github.com/novacrazy/bluebird-co) 是一个高效的 yield 处理程序，
该处理程序可以转换与 [tj/co](https://github.com/tj/co) 所有相同的 yieldable 值类型以及更多。

使用

```js
require('bluebird-co');
var myAsyncFunction = Promise.coroutine(function*() {
    var results = yield [
      Promise.delay( 10 ).return( 42 ),
      Promise.resolve( "somefile contents" ),
      [1, Promise.resolve( 12 )]
    ];
    console.log(results); //[42, "somefile contents", [1, 12]]
});

myAsyncFunction().then(...);
```

### Yieldable Types

- Promises
- Arrays
- Objects
- Generators and GeneratorFunctions
- Iterables (like new Set([1, 2, 3]).values())
- Functions (as Thunks)
- Custom data types via [.addYieldHandler(fn)#addyieldhandlerfn--function
- Any combination or nesting of the above.
