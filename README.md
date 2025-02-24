# 什么是 飞将
飞将SSLVPN是一款符合企业级远程办公和安全访问的零信任解决方案。支持远程办公、网络管理、权限管理、应用管理、日志审计、内网穿透等实用功能。部署简单，轻松运维，无需公网IP一键连接内网应用。全终端支持（飞将支持macOS和Windows的自研客户端），本项目借鉴了Ocserv的开发思路，完全兼容思科AnyConnect客户端和OpenConnect客户端。社区版支持对接AD域作为认证源，其他详细信息，[请参阅官网介绍](http://feijiangkeji.com/docs/feijiang/)。

## 特色功能
- 🥳 **<span style="color: #a8b1ff">支持多种方式登录</span>:** 支持用户名和短信验证方式登录
- 🔑 **<span style="color: #a8b1ff">安全性</span>:** 支持新设备验证、水印、日志审计
- 🛠️ **<span style="color: #a8b1ff">适配多种客户端连接</span>:** 适配多种客户端连接(openconnect,思科等)，同时也支持禁用第三方客户端登录
- 🎉 **<span style="color: #a8b1ff">VPN</span>:** 提供了安全的远程办公解决方案，使员工能够在任何地方高效工作，满足现代企业对灵活办公的需求。
- 🎉 **<span style="color: #a8b1ff">应用管理</span>:** 企业常用应用或者网站统一管理、统一入口。
- 🤖️ **<span style="color: #a8b1ff">支持定制化</span>:** 可根据企业需求进行定制化
- 📦 **<span style="color: #a8b1ff">跨平台兼容</span>:** 完美兼容macOS和Windows，无论您身处何种操作系统，我们的产品都能与您完美契合，让您无需受限于设备选择，自由畅享产品的卓越体验！
- 📖 **<span style="color: #a8b1ff">全面详尽的使用文档</span>:** 我们提供清晰易懂的文档支持，涵盖了产品的方方面面，让您能够轻松上手，快速掌握各种技巧，尽情探索产品的无限可能！

# VPN安装操作指引
## 1、下载软件
```shell
wget http://www.feijiangkeji.com/download/feijiang/feijiang-server-installer-0.0.1-linux-amd64.tar.gz

```
- .env: 环境变量
- docker-compose.yaml: docker部署文件
- feijiang-admin: 飞将后台服务
- feijiang-vpn: 飞将VPN服务
- commons:文件夹存放docker部署依赖的配置文件

## 2、部署准备

开始部署之前，请先关注微信公众号【飞将互联】，在菜单中点击产品授权，获取产品授权码
![wechat](/images/wechat.png)


将获取到的授权码添加到.env文件中：
```shell
AUTH_CODE=your_auth_code_here  # 替换为你的实际授权码
```
> [!NOTE]
> 每个微信号只能获取一个授权码



## 3、部署
> [!NOTE]
> 部署支持两种方式


## 3-1、Docker部署(推荐)
修改.env文件的Mongo配置参数，包括以下配置:
```shell
MONGO_HOST=127.0.0.1                     # MongoDB 服务器地址
MONGO_PORT=27017                         # MongoDB 服务器端口
MONGO_USERNAME=feijiang                  # MongoDB 数据库用户名
MONGO_PASSWORD=feijiang_mongo_password   # MongoDB 数据库密码
MONGO_DATABASE=feijiang                  # MongoDB 数据库名称
```
启动容器
```shell
sudo docker-compose up -d
```
> [!NOTE]
> 目前容器部署只支持host模式


### 3-2、本地部署
> [!NOTE]
> 如果已安装MongoDB，请更新.env文件，相关参数见 # 3-1。


更新配置后，启动Feijiang服务
```shell
sudo nohup ./feijiang-admin > feijiang-admin.log &
sudo nohup ./feijiang-vpn > feijiang-vpn.log &
```
> [!NOTE]
> 如果未安装MongoDB，可以使用Docker Compose安装。先修改.env文件Mongo配置参数，相关配置见 # 3-1。


启动mongodb的docker容器

```shell
sudo docker-compose -f commons/composes/feijiang-mongo.yaml --env-file .env up -d
```


### 3-3、访问
- 管理端
```shell
http://你的宿主机ip:8080
```
> [!NOTE]
> 管理后台默认账号密码: admin/admin123!@#



- vpn server
内网与公网ip映射

```shell
你的公网ip:8560
```

### 3-4、版本升级
> [!NOTE]
> 升级前请备份配置文件conf目录和数据库，并停止服务

使用新版本的feijiang-admin二进制文件和feijiang-vpn二进制文件，放到对应的目录下面，替换旧版本。然后重新启动服务。

# 管理端操作指引

## 1、管理端首页
首页展示基础数据的统计，包括账号数，vpn配置地址数，在线会话数等。

![后台首页.png](/images/后台首页.png)

## 2、实名认证
实名认证主要用于管理员可以修改各项飞将系统参数使用，例如下面的内网穿透
![实名认证.webp](/images/实名认证.webp)

## 3、内网穿透
> 飞将提供免费版2个端口和1兆带宽的免费内网穿透服务，可以用于免费用户测试，微企访问内网使用。

- 服务名称：管理员用来区分映射
- 协议类型：支持TCP和UDP
- 内网地址：当映射飞将服务时，填写：127.0.0.1；当映射内网服务器时，请填写内网IP地址
- 内网端口：请填写服务的端口号
- 带宽权重：0比10占比，社区版免费1兆带宽
- 失效时间：此条映射何时失效

![内网穿透.webp](/images/内网穿透.webp)
![内网穿透2.webp](/images/内网穿透2.webp)

## 4、管理端身份管理
分为分组管理、账号管理、身份源设置。

### 4-1、分组管理
为了⽅便账号管理和授权，平台⽀持⾃定义分组，可以通过分组管理 功能新建分组。在功能或资源授权时可以直接选择⾃定义的分组授权。

新增按钮创建分组后，选择组内账号，可以进行新帐号创建或者绑定已有帐号
![后台分组管理.png](/images/后台分组管理.png)

### 4-2、账号管理
支持用户新增、删除、编辑，用户密码修改、用户冻结、账号失效时间
![后台账号管理.png](/images/后台账号管理.png)

### 4-3、身份源设置
支持多个账号来源，可以自建账号，可以从第三方同步等
![后台身份源设置.png](/images/后台身份源设置.png)

## 5、管理端VPN管理
分为访问策略、在线会话、分配地址、其他设置。

### 5-1、访问策略
- 1、基础配置，作用于对象支持分组和单一账号，配置是否允许第三方客户端接入
- 2、DNS配置
- 3、路由设置，支持ip段和域名，包含准入和排除路由设置
- 4、访问控制，支持对网段端口进行策略控制，进一步增加安全性
![后台vpn访问策略.png](/images/后台vpn访问策略.png)

### 5-2、在线会话
支持实时查看在线用户连接情况，可以强制用户下线
![后台vpn在线会话.png](/images/后台vpn在线会话.png)

### 5-3、分配地址
查看每个账号各个终端分配的ip情况，支持删除，重新分配
![后台vpn分配地址.png](/images/后台vpn分配地址.png)



## 6、管理端应用管理
分为应用分类、Web应用。

### 6-1、应用分类
支持应用分类增、删、改、查，方便应用分类进行管理
![后台应用分类.png](/images/后台应用分类.png)

### 6-2、Web应用
支持Web应用录入，将公司内部常用应用或者网站统一管理，统一访问入口，免于记忆，同时可以与企业的SSO相结合实现业务系统免登录(需配合企业场景定制开发)。
后续会接入个性化水印设置。
![后台应用管理.png](/images/后台应用管理.png)


## 7、管理端终端管理
分为设备列表、授权记录。

### 7-1、设备列表
展示所有用户登录的设备列表详情
![后台终端设备列表.png](/images/后台终端设备列表.png)

### 7-2、授权记录
展示所有用户授权的设备，可以自动或者手动授权设备，同时也可以禁用相应设备
![后台授权记录.png](/images/后台授权记录.png)

## 8、管理端日志审计
分为VPN日志、终端登录日志、后台系统日志。

### 8-1、VPN日志
- 1、活动日志:用户登录、退出、认证等日志信息
- 2、访问日志:用户访问日志
![后台终端vpn日志.png](/images/后台终端vpn日志.png)
### 8-2、终端登录日志
终端设备登录日志详情
![后台终端登录日志.png](/images/后台终端登录日志.png)

### 8-3、后台系统日志
- 1、登录日志：记录后台系统登录日志
- 2、操作日志：记录后台系统操作日志
![后台系统日志.png](/images/后台系统日志.png)


# 客户端操作指引
## 1、飞将客户端登录
企业成员在桌面双击登录，会出现登录页面，根据管理员提供的账号和密码进行登录即可！
> [!NOTE]
> 修改网关地址:网关地址写你的服务器公网IP地址，或者端口映射后的公网IP，如果是使用飞将的内网穿透，请填写飞将提供的公网IP和端口（飞将VPN端口默认TCP8560）。


![客户端-登录.webp](/images/客户端-登录.webp)
![客户端-后端地址配置.webp](/images/客户端-后端地址配置.webp)


## 2、VPN介绍
如果你不在公司，需要进入公司内网网址进行远程办公，可点击按钮，开启vpn
> [!NOTE] 
> 如果你已经在公司内网环境，则默认显示在内网环境，无需操作。


未连图片
![vpn断开.png](/images/vpn断开.png)

连后图片 
![vpn打开.png](/images/vpn打开.png)










