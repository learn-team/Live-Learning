##自定义函数

####less

        .border-radius (@radius) {
            border-radius: @radius;
            -moz-border-radius: @radius;
            -webkit-border-radius: @radius;
        }
        #header {
            .border-radius(4px);
        }
        
####sass

        @mixin border-radius(@radius) {
            border-radius: @radius;
            -moz-border-radius: @radius;
            -webkit-border-radius: @radius;
        }
        #header {
                @include border-radius(4px);
        }

##变量声明

####less
        
        $var: red;
        
####sass

        @var: red;
        
##变量作用域

####less

因为 less “按需加载”（lazy loaded）的，因此不必强制在使用之前声明，如下  是有效的：

    lazy-eval {
        width: @var;
    }
    @var: @a;
    
而 sass 必须按照代码流声明变量，否则会报错，如下则编译失败：
    
    lazy-eval {
        width: $var;
    }
    $var: red;

##继承类

    .class {
      color: red;
    }
  
####less
  
    h1{
      .class;
    }

####sass

    h1{
      @extend .class;
    }
