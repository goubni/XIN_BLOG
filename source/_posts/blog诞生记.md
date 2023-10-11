---

title: 建站教程
excerpt: 过程坎坷，没有耐心debug可不好搭

---

#建站过程

- 记录一下，我刚上大二，对于一些java、python的开发还有前端都有所涉猎，可能是debug比较有耐心吧，花了整整两天才把这个blog搭起来，而且我也不是完全参照原文，中途还有更换主题等操作。过程相当坎坷，需要有耐心

- 搭一个blog对于很多刚入这行的肯定很困难，但是有教程就会简化这个过程，我参考的是[个人博客搭建教程 | 爱扑bug的熊 (cuijiacai.com)](https://blog.cuijiacai.com/blog-building/)的博客搭建教程，非常的清晰，不过里面每一个步骤还隐藏了一些细节，光靠这一个教程还是很难搭起来的，搭建过程中会出现许多奇奇怪怪的问题，这个时候就需要擅用[AIchatOS (jinshutuan.com)](https://chat2.jinshutuan.com/#/chat/1696922105444)了。不懂的就cv甩给他。

> 总结一下，搭建这个网站的主要逻辑就是：用hexo框架生成博客，github上托管自己的项目文件，在netlify上部署github上的静态网页，之后用阿里云购买域名，解析netlify生成的网址，最后用cloudFlare加速。之后只要在本地修改文件，在上传到github，站点就会帮你自动更新了！！！😉


