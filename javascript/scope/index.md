## JavaScript 作用域



### 作用域是什么？


#### 负责收集并维护由所有声明的标识符(变量)，并实施一套非常严格的规则，确定当前执行的代码对这些标识符的访问权限。

#### 换句话说，就是 [js引擎]() 执行到当前代码区域，可以访问到的标示符（变量）。


下面直接给你整的明明白白：
```javascript
    //❶全局作用域
    function foo(a){
        //❷foo作用域  
        var b = a * 2;
        function bar (c){
            //❸bar作用域
            console.log(a,b,c);
        }
        bar(b*2);
    }
    foo(2); // 2 4 8    
```
### 创建作用域链
    ❶ 包含着整个全局作用域，其中只有一个标识符:foo。
    ❷ 包含着 foo 所创建的作用域，其中有三个标识符:a、b 和 bar。   
    ❸ 包含着 bar 所创建的作用域，其中只有一个标识符:c。  

作用域会先找到谁？

```javascript
    //❶全局作用域
    var a= 1;
    function foo(){
        //❷foo作用域  
        var a = 2;
        console.log(a)// 2
    }
    foo() 
```


```javascript
 // ❶全局作用域
    foo(a);
    console.log(a,b,c) //？？？ 全局作用域能访问到吗
```

    ❶ 全局作用域是无法访问❷foo❸bar作用域的成员变量····抛出异常了

### 函数作用域和块级作用域
```javascript
    //函数作用域
    function fn(){
        var a = 2
    }
    fn()
    console.log(a) // Uncaught ReferenceError
```
```javascript
    //块级作用域
    if(true){
        let a = 100
        var b = 200
    }
    console.log(a) // Uncaught ReferenceError
    console.log(b) // 200
```
    函数作用域可以把他当作一个受保护的作用域，块级作用域在里面var声明会声明作用域的（函数或全局）。

## 总结
作用域很好理解，要JS引擎执行代码给你创建了作用域链，当你访问一个变量，首先会在自己这一块区域找，找不到往上一块包含区域找。找不到就undefined...

在这里我没有去讲“词法作用域”、“动态作用域”，其实就是自己代码区域叫词法作用域，动态作用域就是不确定性还要去找。

还有一个小提示：如果是找a = 1这种赋值情况下没找到a，那么会在全局作用域上创建a变量并赋值。

JavaScript 中有两个机制可以“欺骗”作用域:eval(..) 和 with。
这两个机制的副作用是引擎无法在编译时对作用域查找进行优化，因为引擎只能谨慎地认为这样的优化是无效的。使用这其中任何一个机制都将导致代码运行变慢。不要使用它们。


### 下一个知识点
#### [《JavaScript动态作用域》](./dynamic.md)
#### [《JavaScript变量函数提升》](../promote/index.md)