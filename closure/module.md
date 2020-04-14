## JavaScript 闭包应用(模块)


### 模块
```javascript
function foo() {
  var something = "cool";
  var another = [1, 2, 3];
  function doSomething() {
    console.log(something);
  }
  function doAnother() {
    console.log(another.join(" ! "));
  }
  return{
    doSomething:doSomething,
    doAnother:doAnother
  }
}

var foo = foo()

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

这个模式在 JavaScript 中被称为模块。最常见的实现模块模式的方法通常被称为模块暴露， 这里展示的是其变体。
如果要更简单的描述，模块模式需要具备两个必要条件。
1. 必须有外部的封闭函数，该函数必须至少被调用一次(每次调用都会创建一个新的模块实例)。
2. 封闭函数必须返回至少一个内部函数，这样内部函数才能在私有作用域中形成闭包，并且可以访问或者修改私有的状态。


一个具有函数属性的对象本身并不是真正的模块。从方便观察的角度看，一个从函数调用所返回的，只有数据属性而没有闭包函数的对象并不是真正的模块。

当只需要一个实例时，可以对这个模式进行简单的改进来实现单例模式
```javascript
var foo = (function foo() {
  var something = "cool";
  var another = [1, 2, 3];
  function doSomething() {
    console.log(something);
  }
  function doAnother() {
    console.log(another.join(" ! "));
  }
  return{
    doSomething:doSomething,
    doAnother:doAnother
  }
})()

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

模块也是普通函数，也可以接受参数
```javascript
function person(name){
    function say(){
      console.log(name)
    }
    return {
      say:say
    }
}

var kip = person("Kip Song")
kip.say()

var dora = person("Dora")
dora.say()
```

模块模式另一个简单但强大的变化是，命名将要作为公共API返回对象：

```javascript
function kipsong(age){
    var myself = {
      name:"kip song",
      age:age,
      say:say,
      growUp:growUp
    }
    function say(){
      console.log("My name is " + myself.name + ", I am " + myself.age + " years old")
    }
    function growUp(){
      myself.age +=1
    }
    return myself
}

var kip = kipsong(23)
kip.say()// My name is kip song, I am  23 years old
kip.growUp() //我长大1岁了！
kip.say()// My name is kip song, I am  24 years old
```