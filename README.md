移动端前端解决方案
==========================================

## 目录

- 架构篇
    - 介绍
        + 前端工程化
        + 前端项目开发流程
    - 静态资源管理
        - 介绍
        - 目录规范
        - 组件化
        - 技术选型
        - 静态资源加载
            - 按需加载静态资源
    - 静态资源缓存
    - Quickling方案
- 应用篇
    + ...
- 安全篇
    + XSS
    + 跨域

《移动端前端解决方案》解决移动Web开发中遇到的工程、应用、安全问题，阐述使用FIS如何快速解决这些问题以及如何做是最优雅、最高效的。

编搞人：
    - @2betop
    - @xiangshouding
    - @wangcheng
    - @zhangying
    - 待定


----
TODO list

## 架构篇

### 静态资源加载 @xsd
- [ ] 静态资源压缩、优化 √
- [ ] 静态资源合并 √
- [ ] combo服务 -
- [ ] CDN部署 √

### 浏览器静态资源缓存 @zy
- [ ] localStorage缓存 diff/async - @wc,@xz
- [ ] 浏览器强缓存 √
- [ ] minifest √

### 模板 @xsd

- [ ] 前端模板渲染 √
- [ ] 后端模板渲染 √

### 组件化 @xz

- [ ] cmd -
- [ ] amd √

### 无线离线存储 -
- [ ] IndexedDB
- [ ] webdb

## 应用篇

### 公共库、手势库、组件的选择
- [ ] 选择原则 

### 图片、视频 @xz
- [ ] 不同屏幕大小下的图片 √

### 动画 √ @xz

- [ ] setIntervel / setTimeout
- [ ] css3 animation
- [ ] frame animation

### 字体 -
- [ ] 移动端的字体选择
- [ ] 图标做成字体

### 响应式 -

### SEO -
- [ ] WebApp的SEO优化

### 客户端交互 -
- [ ] 添加到桌面
- [ ] 客户端调起
- [ ] 电话调起
- [ ] 键盘类型控制

### 浏览器兼容 √
- [ ] 常见兼容性问题列表

### 机型、浏览器适配 √ @zy
- [ ] wise

### 微信相关 -

### 统计 √ @xsd

### 广告 -

## 安全篇 √ @xsd

### 跨域

### XSS

## 附件

一些需要参考的网站

 - https://developers.google.com/webmasters/smartphone-sites/details
 - https://developers.google.com/web/fundamentals/
 - http://fedev.baidu.com/~tieba/docs/mo-guidebook/
 - http://caniuse.com/#feat=indexeddb
