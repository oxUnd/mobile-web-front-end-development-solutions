
## 页面加载控制的优化

页面加载的各种性能优化手段，分别是`bigrender`、`bigpipe`、`lazyrender`。

- `bigrender` 一种减少首屏dom节点优化首屏展现的方案；
- `bigpipe`  一种通过chunk输出来优化网页展现的方案；
- `quickling` 一种加载页面的模式，页面的一个局部可以通过异步请求，请求数据包括渲染好的页面以及静态资源；
- `lazyrender` 手机上可能网络传输很慢，如果先传输一部分过来展示，然后再根据用户的操作输出剩下的部分，无疑是可以提高首屏时间的。整个方案就命名为`lazyrender`

### bigrender

详细说一下bigrender如何实现的，一个页面可能占几屏，首先出现在用户视野里的自然是第一屏。一次渲染如果把所有都渲染，无疑需要比较长的时间，这时候用户无法操作界面甚至于无法看到页面。bigrender的思路是这样的；

+ 先渲染首屏，其余的部分的html被放到注释里面（或`textarea`），这样就保证不被渲染，较少首屏时间。
+ 当用户向下查看次屏的内容时，再触发脚本渲染次屏的内容。
+ 依次展现整个网页

### bigpipe

bigpipe对服务器要求比较高，需要支持chunk输出。其实就是在同一个链接上将一个页面分段输出。需要服务器端能并行处理页面，假设页面可以分为三屏，没屏都对应一些数据。当某一屏的数据获取成功，其可以马上返回给浏览器（也就是pipe的方式输出），这样对应的屏就展现出来了。如果支持并行处理，再以chunk的方式输出，无疑会让浏览器加载展现会快很多。

对于上面说的举个例子；假设页面分为三部分A，B，C，A的数据是调用后端服务层`api_a`得到，B是`api_b`，C是`api_c`。

_node.js示例_

```javascript
var asyncMap = require('slide').asyncMap;
var app = require('express')();

app.get('/', function (req, res) {
    //并行处理
    asyncMap(
        [
            "http://api.xxx.com/api_a",
            "http://api.xxx.com/api_b",
            "http://api.xxx.com/api_c"
        ],
        function(url, cb) {
            http.get(url, function(r) {
                var d = '';
                r.on('data', function (c) { d += c.toString(); });
                r.on('end', function() {
                    res.write(d); //直接输出
                    cb(null, '');
                });
            });
        },
        function(err, r) {
            res.end();
        };
    );
});

app.listen(3000);
```
上面就是一个并行化的例子，当然了如果不是api调用，查询数据库也是同样的道理。

可能会有这样的疑问，如何精准控制页面的展现。比如可能第二屏先出来，导致用户看到的是第二屏，而第一屏跑到了第二屏下面；其实这个已经有成型的解决方案了，比如facebook，微博等都实现了bigpipe。

解决上面问题的大致思路是，先给浏览器吐一个结构。

```html
<html>
    ...
    <div id="first"></div>
    <div id="second"></div>
    ...
</html>
```
然后在html结束后，输出一些js代码来渲染内容上去；比如

```html
<html>
    ...
    <div id="first"></div>
    <div id="second"></div>
    ...
</html>
<script>render('first', function() {/*first内容*/});</script> <!-- 第一次chunk -->
<script>render('second', function() {/*second内容*/});</script> <!-- 第二次chunk -->
```
这样就完美搞定页面渲染的问题，并且提升了不少性能，一个部分一个部分渲染的嘛~

### lazyrender

在手机网络下，html如果太大，会传输很长时间。这时候如果把页面拆解为几部分然后分块请求，将有利于页面的展示。当然了，如果后端比较龟速，分成几块也可以使用相同的方式去加快页面的展现。

lazyrender就是这个思路。大概做法就是，当页面渲染的时候，把首屏先传输给浏览器进行展示，其他屏或者不重要的东西给页面打一段js。大概是这个样子的；

```html
<html>
    ...
    <div>首屏代码</div>
    <script>lazyrender('second');</script>
    ...
</html>
```

当首屏展现完成后，可以触发次屏的加载。使用quickling的方式加载次屏的html，css，js等然后进行渲染。

### 小结

- 介绍了现在主流的一些页面加载控制来提高性能的方案及其原理