---
title: node-sass安装配置命令
date: 2023-11-02 10:50:03
description: node-sass配置命令便捷方式
tags:
- 踩坑记录 
categories:
- 踩坑记录 
---

<h3>方法一（没成功）</h3>
<ol>
    <li>npm或yarm指定淘宝镜像</li>
    <li>安装windows平台编译环境（需要在管理员权限下安装）</li>

```shell
npm install -g node-gyp
npm install --global --production windows-build-tools
``` 

<li>在项目目录下临时安装指定node-sass为镜像淘宝</li>

```shell
npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
```

</ol>

<h2>实际解决流程</h2>
<h4>按照方法一走了一遍没成功，然后下载安装python2，pc的react项目成功，小程序的taro项目失败</h4>
<h4>小程序报错 spawn C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\MSBuild\15.0\Bin\MSBuild.exe
ENOENT</h4>
<h4>运行指令</h4>

```shell
npm config set msbuild_path "C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\MSBuild\Current\Bin\MSBuild.exe" -g
```

<h4>小程序项目成功</h4>
