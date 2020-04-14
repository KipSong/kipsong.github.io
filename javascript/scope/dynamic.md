## JavaScript 动态作用域

### 动态作用域就是这个
```javascript
function foo() {
  console.log( a ); // 2
}
function bar() { 
  var a = 3;
  foo(); 
}
var a = 2;
bar();
```
很多人就不明白了，根据上面所讲的“词法作用域”到这里怎么感觉不对了呀。不应该是 3 吗？

### 再回去一趟 我们聊聊词法作用域

```javascript
function bar() { 
  var a = 3;
  function foo() {
    console.log( a ); // 3
  }
  foo(); 
}
var a = 2;
bar();
```
在这里我们都知道是 3。


## 解释一下
主要区别:词法作用域是在写代码或者说定义时确定的，而动态作用域是在运行时确定的。(this 也是!)词法作用域关注函数在何处声明，而动态作用域关注函数从何处调用。

最后，this 关注函数如何调用，这就表明了 this 机制和动态作用域之间的关系多么紧密。

#### [《JavaScript变量函数提升》](../promote/index.md)