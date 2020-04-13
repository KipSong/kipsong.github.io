## JavaScript 提升



##### 函数作用域和块作用域的行为是一样的，可以总结为：任何声明在某个作用域内的变量，都将附属于这个作用域。



##### 但是作用域同其中的变量声明出现的位置有某种微妙的联系

#### 变量声明
不多说了直接看代码

```javascript
  a = 2;
  var a; 
  console.log( a );
```
  懵逼选手肯定有很多，到底是什么？
  在这里我先解读一下
  第一步：var a 声明变量提升 undefined
  第二步：a = 2
  第三部：console.log(a)
  答案是：2


另外再考虑一段代码
```javascript
  console.log( a );
  var a = 2;
```
新手又懵逼了？
第一步：var a 声明变量提升 undefined
好了你知道答案了

##### 为了搞明白这个问题，这里其实是js引擎编译器的内容。JavaScript 实际上会将其看成两个 声明:var a;和a = 2;。第一个定义声明是在编译阶段进行的。第二个赋值声明会被留在 原地等待执行阶段。

```javascript
  // 为第一段代码进行处理
  var a; 
  a = 2;
  console.log( a );

  // 为第二段代码进行处理
  var a;
  console.log( a );
  a = 2;
```
---
#### 函数声明
我们再来看看，这里大家都懂的函数声明提升
```javascript
  foo();
  function foo() {
    console.log( a ); 
    var a = 2;
  }
  // 我们给上面代码处理一下
  foo();
  function foo() {
    var a；
    console.log( a ); // undefined
    a = 2;
  }
```

再看一下函数表达式声明：不可提升
```javascript
  foo(); // 不是 ReferenceError, 而是 TypeError!
  var foo = function bar() {
    // ...
  };

```
分析一下：
第一步：var foo 声明变量提升 undefined
第二步：执行了foo( ) 你说呢？ 执行undefined( )； TypeError。。。

---
#### 函数优先
函数声明和变量声明都会被提升。但是一个值得注意的细节(这个细节可以出现在有多个
“重复”声明的代码中)是函数会首先被提升，然后才是变量。
```javascript
  foo(); // 1
  var foo;
  function foo() {
     console.log( 1 );
  }
  foo = function() {
     console.log( 2 );
  };
  // 我们来给他捋一捋

  function foo() {
    console.log( 1 );
  }
  foo() // 1
  foo = function() {
     console.log( 2 );
  };
```
注意，var foo 尽管出现在 function foo()... 的声明之前，但它是重复的声明(因此被忽略了)，因为函数声明会被提升到普通变量之前。

```javascript
  // 再来看看这段操作 
  foo(); // 3
  function foo() { 
    console.log( 1 );
  }
  var foo = function() {
    console.log( 2 );
  };
  function foo() { 
    console.log( 3 );
  }

```
尽管重复的 var 声明会被忽略掉，但出现在后面的函数声明还是可以覆盖前面的。

这是很糟糕的写法。(现在已修复，来自《你不知道的javascript》)

```javascript
foo(); // "b"
var a = true;
if (a) {
  function foo() {
   console.log("a");
  }
}else {
  function foo() { 
    console.log("b"); 
  }
}
```
### 总结
首先，函数声明（非函数表达式）提升大于变量声明，理解var 声明分为两个阶段，提前声明到作用域为undefined，然后再进行赋值。

这里我给一个有意思的代码
```javascript
  var a = 'global'
  function main(){
    console.log(a)
  }
  main()
```
```javascript
  var a = 'global'
  function main(){
    console.log(a)
    var a = 'private'
  }
  main()
```
结合作用域的知识，看看你的答案对不对。
// global
// undefined

没理解的朋友，我肯定给你整明白。
直接大白话讲，我们查询a，第一步是找自己作用域，没有就往作用域链上面继续找。
看！第二个例子，var a 变量提升 undefined。它找到了它不装了。