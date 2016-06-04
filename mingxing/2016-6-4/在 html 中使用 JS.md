##JavaScript 高级程序设计( 第 3 版 )

####外部文件优势

<b>可维护性：</b>遍及不同 HTML 页面的 JS 会造成维护问题，但将 JS 放入一个文件则方便管理，并且也能够不触及 HTML 标记而集中精力遍及 JS 代码。

<b>可缓存：</b>通过浏览器对一个 JS 文件进行缓存，当其他页面在使用到此文件，则不需要再次加载。

<b>适应未来：</b>不需要因为 XHTML 与 HTML 的切换使用 hack，两种规范使用的外部文件语法是相同的。

####defer 延迟脚本

&lt;script> 的 defter 属性的用途是表明脚本在执行时不会受页面的构造而影响加载，也就是说，脚本会被延迟到整个页面都解析完毕后再运行。

    <script type="text/javascript" defer="defer" src="example.js" ></script>

####async 脚本异步加载

与 defer 类似，都用于改变处理脚本的行为。不同的是 async 的脚本不保证按照文档流的顺序执行。

    <script type="text/javascript" async="async" src="example1.js" ></script>
    <script type="text/javascript" async="async" src="example2.js" ></script>
    
实例中 `example2.js` 可能在 `example1.js` 前加载。

特点：

1. 不必等待其它脚本，也不必阻塞文档呈现。

2. 不能保证异步脚本按照它们在页面中出现的顺序执行。

####&lt;noscript> 元素

如果浏览器不支持脚本，或者浏览器禁止了脚本，那么 &lt;noscript> 中的内容将被显示

    <noscript>
        本页面需要浏览器支持( 启用 ) JS。
    </noscript>
    
####总结

由于浏览器会先解析完不适用 defer 的外部脚本文件中的代码，然后在解析后面的内容。所以为了避免页面渲染延迟和空白，通常将脚本置于 &le;/body> 元素前( 即页面尾部 )。
