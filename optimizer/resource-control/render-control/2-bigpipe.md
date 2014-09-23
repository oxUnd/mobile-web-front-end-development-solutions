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