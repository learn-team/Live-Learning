函数实际上是对象，每个函数都是 Function 类型的实例，而且与其他引用类型一样具有属性和方法。

由于函数式对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。函数通常是使用 `函数声明` 语法定义的，如下：

  function sum( num1, num2){
    return num1 + num2;
  };
  
这与下面使用 `函数表达式` 定义函数的方式几乎相差无几。

  var sum = function( num1, num2){
      return num1 + num2;
  };
