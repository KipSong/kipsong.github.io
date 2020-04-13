## JavaScript 闭包



### 闭包是什么？
##### 当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行。

#### 换句话说，就是这个函数执行，可以记住并可访问到所在词法作用域。
#### 再换句话说，就是函数放在哪执行，原函数词法作用域保持完整。


```javascript
  function foo(){
    var a = 2;
    function bar(){
      console.log(a);
    }
    return bar
  }
 var bar = foo();
 bar();//2 这就是闭包的效果
```
    在 foo() 执行后，通常会期待 foo() 的整个内部作用域都被销毁，因为我们知道引擎有垃圾回收器用来释放不再使用的内存空间。由于看上去 foo() 的内容不会再被使用，所以很自然地会考虑对其进行回收。

    而闭包的“神奇”之处正是可以阻止这件事情的发生。事实上内部作用域依然存在，因此没有被回收。谁在使用这个内部作用域?原来是 bar() 本身在使用。
    拜 bar() 所声明的位置所赐，它拥有涵盖 foo() 内部作用域的闭包，使得该作用域能够一直存活，以供 bar() 在之后任何时间进行引用。

    bar() 依然持有对该作用域的引用，而这个引用就叫作闭包。

```javascript
function foo() {
  var a = 2;
  function baz() {
    console.log( a ); // 2
  }
  bar(baz);
}
function bar(fn) {
  fn(); // 这就是闭包!
}
foo();
```
    把内部函数 baz 传递给 bar，当调用这个内部函数时(现在叫作 fn)，它涵盖的 foo() 内部作用域的闭包就可以观察到了，因为它能够访问 a。
```javascript
var fn;
function foo() {
  var a = 2;
  function baz() { 
    console.log( a );
  }
  fn = baz; // 将 baz 分配给全局变量 
}
function bar() {
  fn();//闭包
}
foo();
bar(); // 2
```
###### 无论通过何种手段将内部函数传递到所在的词法作用域以外，它都会持有对原始定义作用域的引用，无论在何处执行这个函数都会使用闭包。

##### 其实哪里都是闭包应用

###### setTimeout
```javascript

function wait(message) {
  setTimeout( function timer() {
     console.log( message );
  }, 1000 );
 }
wait( "Hello, closure!" );
```
这里很多人就感觉懵逼为什么就是了？
那我们结合什么叫作用域的定义来分析一下，timer函数作用域所引用的message变量是wait作用域里的。引擎执行timer延迟1000ms依然是“Hello, closure!”，而词法作用域在这个过程中保持完整。

###### JQuery
```javascript
function setupBot(name, selector) {
  $( selector ).click( function activator() {
    console.log( "Activating: " + name ); 
  } );
}
setupBot( "Closure Bot 1", "#bot_1" );
setupBot( "Closure Bot 2", "#bot_2" );
```
#### 监听器、 Ajax 请求、跨窗口通信、Web Workers 或者任何其他的异步(或者同步)任务中，只要使 用了回调函数，实际上就是在使用闭包! 
#### 这个时候再回到上面看定义，什么是闭包。

###### IIFE 模式。通常认为 IIFE 是典型的闭包例子 IIFE是闭包么？
```javascript
var a = 2;
(function IIFE() { 
  console.log( a );
})();
```
###### 这里其实不是，它在定义时所在的作用域中执行(而外部作用域，也就是全局作用域也持有 a)。a 是通过普通的词法作用域查找而非闭包被发现的。

### 待续