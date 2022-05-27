# 项目介绍

Go 微服务网关是一个由我个人搭建的前后端分离的微服务网关服务，这是配套的前端代码。

该前端代码由vue(2.0)-element-admin修改而来，里面仍存有一些没用上但没删除的代码，以便后续开发。

技术栈：vue2.0+element

前端项目：https://github.com/uptocorrupt/go_gateway_view

对应后端项目：https://github.com/uptocorrupt/go_gateway

联系邮箱：hhd5050@foxmail.com

# 项目预览

[![1.png](https://i.postimg.cc/7hr9y85j/1.png)](https://postimg.cc/5YpLwrqm)

[![3.png](https://i.postimg.cc/7LXBvY7c/3.png)](https://postimg.cc/B8L5H3mc)

[![2.png](https://i.postimg.cc/CM2QL1tp/2.png)](https://postimg.cc/CdjNcFSc)

# 实现功能

管理员：登录、退出、修改密码

大盘：流量统计展示、全站服务类型占比、四维度指标

租户：管理列表、租户信息curd、流量统计

服务：服务列表、流量统计、服务信息crud


# 代码帮助

- 首先git clone 前端项目 

```
git clone https://github.com/uptocorrupt/go_gateway_view.git
```

- 确保本地环境安装了nodejs

```
node -v
```

- 安装node依赖包

```
cd go_gateway_view
npm install
```
​	或者使用国内代理
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install
```

- 运行前端项目

```
npm run dev
```



