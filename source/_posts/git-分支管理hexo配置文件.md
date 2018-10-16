title: git 分支管理hexo配置文件
author: jelly
date: 2018-05-23 11:48:57
tags:
---
###### git push 本地博客文件夹 
1、/Users/JellyB/Documents/Blog 内容到github 分支 hexo 下

``` bash

cd /Users/JellyB/Documents/Blog
git init  //初始化本地仓库
git add . //将必要的文件依次添加
git commit -m "提交hexo 配置文件"
git branch hexo  //新建hexo分支
git checkout hexo  //切换到hexo分支上
//将本地与Github项目对接
git remote add origin https://github.com/JellyB/JellyB.github.io.git  
git push origin hexo  //push到Github项目的hexo分支上

```

如果不成功，你是否忘了添加注释?

```
git commit -m ''
```


###### 另一终端（电脑）  git pull hexo 并简单配置

2、clone & setting

``` bash

cd /User/biguodong/Document/Blog
git clone -b hexo https://github.com/JellyB/JellyB.github.io.git  //将Github中hexo分支clone到本地
cd  yourname.github.io  //切换到刚刚clone的文件夹内
npm install    //注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再init
hexo s  //启动本地服务器，使用admin管理博客

```

[Blog](http://localhost:4000)<br>
[Blog-admin](http://localhost:4000/admin)

link ：[CSDN](https://blog.csdn.net/Monkey_LZL/article/details/60870891)