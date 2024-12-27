---
title: How to deploy a blog of your own
author: LYK
date: 2024-11-27 19:00:00 +0800
categories: [Writing, Tools]
tags: [Tools, Blog, Writing, Jekyll, Github pages, Cloudflare]
pin: false
published: true
description: 如何从头部署一个属于你自己的博客？
---


**工具**    
博客  
- 博客服务部署：[Github pages](https://pages.github.com/)
- 样式风格：[Jekyll-theme-Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 

图床  
- 图床部署：[Cloudflare](https://www.cloudflare.com/zh-cn/)
- ~~图床管理：[Telegraph-Image](https://github.com/cf-pages/Telegraph-Image)，由于滥用，已被Cloudflare封禁，图片只能访问一天。~~ 
- 图床管理：[CloudFlare-ImgBed](https://github.com/MarSeventh/CloudFlare-ImgBed)
- 图片存储：[Telegram](https://telegram.org/)

流量监控  
- 流量监控：[Google Analytics](https://analytics.google.com/)


**经验**
```angular2html
启动命令： bundle exec jekyll serve
可选参数：
- --unpublished # 渲染未发布的blog，便于本地调试
```




**Reference**  

- [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](https://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
- [2024年最全的图床集合（国内外，站长必备）](https://juejin.cn/post/7326268998490849289)
- [Telegraph-Image 免费图床搭建全攻略，让你的图片存储更高效！](https://blog.csdn.net/qq_52475653/article/details/134725529)
- [Setting up Google Analytics ](https://github.com/cotes2020/jekyll-theme-chirpy/issues/150)
- [Jekyll 中的配置和模板语法](https://gist.github.com/hellokaton/f88be58ef4ae0f3741bb36ab8daa53c5)
- [Jekyll使用教程笔记 二](https://blog.csdn.net/weixin_34085658/article/details/91476463)





