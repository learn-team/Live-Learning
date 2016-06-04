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
    
####函数声明与函数表达式的区别

解析器在向执行环境中加载数据时，对函数声明和函数表达式并非一视同仁。解析器会率先读取函数声明，并使其在执行任何代码之前可用( 可用访问 )；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行：

    fnSum( 10, 10);     //20
    function fnSum( num1, num2){
        return num1 + num2;
    }
    
以上代码完全可用正常运行，因为在代码开始执行之前，解析器就已经通过一个名为函数声明提升( function declaration hoisting )的过程，读取并将函数声明添加到执行环境中。 对代码求值时，JavaScript 引擎在第一遍会声明函数并将它们放到源代码树顶部。所以，即使声明函数的代码在调用它的代码后面，JavaScript 引擎也能把函数声明提升到顶部。

    fnSum2( 10, 10);    //  Uncaught TypeError: fnSum2 is not a function
    var fnSum2 = function ( num1, num2){
        return num1 + num2;
    }
