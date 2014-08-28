# 静态资源加载

- 介绍
- 静态资源压缩
- 静态资源合并
- CDN


## 介绍

Web前端开发中所指的静态资源一般是包括但不限于html、css、js、图像等资源。而css、js、图像影响着整个页面的展示。所以，合理优化这些资源的加载，会极大提升用户体验。

对于移动端，由于对流量很敏感，静态资源的优化就尤为重要了。

这节主要介绍一下静态资源加载需要做的一些事情以及要解决这些问题，如何通过工具来解决。

原则：

- 静态资源需要压缩、合并
- 静态资源尽量使用CDN

## 静态资源压缩
- js文件压缩
    + js压缩一般选用[UglifyJS]()作为压缩工具
- css文件压缩
    + css压缩一般选用[clean-css]()作为压缩工具
- html文件压缩
    + html建议不要压缩
        * 收益非常非常小。大多数产品线线上都开启了gzip，线下压缩相当多余，最后送达到用户浏览器的html大小没差别
        * 现在没有一个比较好的压缩工具能保持压缩前后效果相同
- 图像压缩
    + `pngquant`和`pngcrush`用来压缩`png`图片
    + `jpegtran`用来压缩`jpeg`图片

FIS中已经集成了各种压缩工具，只需要在编译是添加编译选项`-o`即可。如，

```bash
fis release -r <to-project-path> -o -d <to-output-path>
```

## 静态资源合并

移动端少一个请求，能减少渲染时间，所以合并静态资源是优化性能的重中之重。其中包括js，css，图片等的合并。

合并资源即把多个文件合并成一个文件，并在也面使用这个合并后文件。一般都可以通过手动合并或者脚本合并，但这块主要说的是通过fis来做合并。

### JS/CSS 合并
-

### 图片合并

图片合并的原理是利用`background-position`，把一些小图合并如一张大图后，使用`background-position`来指定显示合并如大图中的小图；俗称`csssprite`。


## CDN
一般都会上CDN，CDN详细请参见[百科](http://baike.baidu.com/link?url=bko8Ek_Gki3nN8L5XWFmDqzxAcrhWoC3n9AHC4XVoTa_u31jp35Xg9Ik7h4w7ioOAamucCrZfSHO0k1OI-zwIq)

除了上面说到的一些优点，主要可以越过浏览器并发请求限制（每一个域名只能发起几个请求）。

fis中处理CDN很简单；

- 配置
    
    ```javascript
    fis.config.set('roadmap.domain', 'http://cdn.test.com');
    ```

- 编译时加参数`-D`

    ```bash
    fis release -D -d <to-output-path>
    ```

这样的操作后，就会在所有的资源前面添加cdn域名；