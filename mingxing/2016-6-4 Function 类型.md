函数实际上是对象，每个函数都是 Function 类型的实例，而且与其他引用类型一样具有属性和方法。

由于函数式对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。函数通常是使用 `函数声明` 语法定义的，如下：

    function sum( num1, num2){
      return num1 + num2;
    };
  
这与下面使用 `函数表达式` 定义函数的方式几乎相差无几。

    var sum = function( num1, num2){
        return num1 + num2;
    };

####没有重载( 深入理解 )

将函数名想象为指针，也有助于 理解为什么 ECMAScript 中 `没有函数重载` 的概念，如下：

    var add = function( num ){
        return num + 100;
    };
        
    var add = function( num ){
        return num + 200;
    };
      
    add( 100 ); //300
    
通过观察重写之后的代码，很容易看清楚到底是怎么回事儿--在创建第二个函数时，实际上覆盖了引用第一个函数的变量 add。 所以 `函数声明` 的函数也是同理。
    
