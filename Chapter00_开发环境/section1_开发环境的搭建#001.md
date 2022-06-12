# 0.1 开发环境的搭建

## 0.1.1 下载 Golang 安装包安装

官网下载稳定版安装包：[Downloads - The Go Programming Language](https://golang.google.cn/dl/)

## 0.1.2 设置环境变量

右击*我的电脑*-->*属性*-->*高级系统设置*-->*高级*-->*环境变量*

将 **GOPATH** 更改为自己的工作区路径。

## 0.1.3 设置插件代理

将代理设置到 [GOPROXY.IO](https://goproxy.io/zh/)

    go env -w GO111MODULE=on
    go env -w GOPROXY=https://goproxy.io,direct
    go clean --modcache

## 0.1.4 创建工作区目录结构

在工作区目录 **GOPATH** 下创建：
- bin：编译后的可执行程序的存储目录。
- pkg：编译时生成的对象文件。
- src：库文件及自己的项目源代码目录。
