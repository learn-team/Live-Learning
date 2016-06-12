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

而以上代码之所以会在运行期间产生错误，原因在于函数位于一个初始化语句中，而不是一个函数声明。换句话说，在执行到函数所在的语句之前，变量 fnSum2 中不会保存有对函数的引用；而且，由于第一行代码就会导致 "Uncaught TypeError" ( 意外的标示符 )错误，实际上也不会执行到下一行。

*除了什么时候可以通过变量访问函数这一点区别之外，函数声明与函数表达式的语法其实是等价的*

####作为值的函数

因为 ECMAScript 中的函数名本身就是变量，所以函数也可以作为值来使用，如引用类型 Array sort 方法：

    var data = [1,2,3,4,24,24,23,42,23,45,67,23,456,65]
    data.sort( compare );
    function compare( a, b ){
        if( a < b ){
            return -1;
        }else if( a > b ){
            return 1;
        }else{
            return 0;
        }
    }

当然也可以从一个函数中返回另一个函数，如下一种极有用的方法：


    var data = [{
        name: 'Zachary',
        age: 28   
    },{
        name: 'Nicholas',
        age: 29   
    }]
    function compare( propertyName ){
        return function( object1, object2){
            var value1 = object1[ propertyName ];
            var value2 = object2[ propertyName ];
            if( value1 < value2 ){
                return -1;
            }else if( value1 > value2 ){
                return 1;
            }else{
                return 0;
            }
        }
    }
    data.sort( compare( 'name' ) ); //Nicholas
    data.sort( compare( 'age' ) ); //Zachary

####函数内部属性

函数内部有两个特殊的对象：arguments 和 this，其中 arguments 除了是保存函数的参数另外还有一个名为 callee 的属性，该属性是一个指向拥有这个 arguments 对象的函数的指针。 如下是一个非常经典的阶乘函数：
    
    console.log( compare( 5 ) ); //120
    function compare( num ){
        if( num <= 1 ){
            return 1;
        }else{
            return num * compare( --num );
        }
    }
    
通常定义阶乘函数会用到递归。以上的函数名是固定的，所以在这种情景之下没有什么不对。但问题是函数内部使用的 compare 名和函数本身紧密耦合在一起，为了清除这种问题，可以将函数改造如下方式：

    console.log( compare( 5 ) ); //120
    function compare( num ){
        if( num <= 1 ){
            return 1;
        }else{
            return num * arguments.callee( --num );
        }
    }
    
在这个重写后的 compare 函数的函数体内，没有在引用函数名。这样无论引用函数时使用的什么名字，都可以保证正常完成递归调用。如：

    var trueCompare = compare;
    compare = function(){
        return 0;
    }
    trueCompare( 5 );   //120
    conpare( 5 );//     0
    
在此变量 trueCompare 获得 compare 的值， 实际上是在另一个位置上保存了函数的指针。如果按照原来函数名得写法这里 trueCompare 就会返回 0，所以使用 callee 就不会出现那样的问题。
    
函数中另外一个特殊的值是 this，与 Java 和 C# 类似，this 引用的是函数执行环境的对象，或者可以说是 this 的值。[ 114 ]

ECMAScript 5 还规范化了另一个函数对象属性：caller，这个属性保存着 `调用当前函数` 的函数引用，比如是在全局作用域中它的值为 null：

    function outer(){
        inner();
    }
    function inner(){
        console.log( arguments.callee.caller );
    }
    outer();
    
*为了加强 JS 这门语言的安全性，让第三方代码不能在相同的环境里窥视其它代码。所以在严格模式下 callee、caller 均会报错*

 ####函数属性和方法
 
 每个函数都包含 length 和 prototype 这两个属性，其中 length 表示函数希望接收的命名参数个数：
 
    function sum( num1, num2){
        return num1 + num2;
    }

以上函数中 sum.length 等于 2。而 prototype 是保存所有实例方法的真正所在，诸如 toString() 和 valueOf() 等方法实际上都是保存在这个属性中。而 prototype 属性是不可以枚举的，因此使用 for-in 无法发现。

每个函数也都包含两个非继承而来的方法：apply() 和 call()。 这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值，如下面实例：

    window.color = 'red';
    var o = {color: 'blue' };
    function fnColor(){
        console.log( this.color );
    }
    fnColor.call( this );   //'red'
    fnColor.call( window );   //'red'
    fnColor.call( o );   //'blue'
    
函数 fnColor 是作为全局定义的，因此 this.color 会转换为是对 window.color 求值，而 fnColor.call( this ) 和 fnColor.call( window ) 则只是两种显式地在全局作用域求值的方式，结果当然就是 'red'，但 fnColor.call( o )不同，函数的执行环境发生了改变，this 被指向了 o，于是结果就是 'blue'。

*使用 call 、 apply 来扩充作用域的最大好处，就是对象不需要与方法有任何耦合关系。*

