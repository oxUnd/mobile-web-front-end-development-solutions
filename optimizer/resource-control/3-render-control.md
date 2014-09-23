
## 页面加载控制的优化

页面加载的各种性能优化手段，分别是`bigrender`、`bigpipe`、`lazyrender`。

- `bigrender` 一种减少首屏dom节点优化首屏展现的方案；
- `bigpipe`  一种通过chunk输出来优化网页展现的方案；
- `lazyrender` 手机上可能网络传输很慢，如果先传输一部分过来展示，然后再根据用户的操作输出剩下的部分，无疑是可以提高首屏时间的。整个方案就命名为`lazyrender`