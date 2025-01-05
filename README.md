# 什么是 飞将
飞将定位是打造一款轻量级的数字化办公平台，具有身份权限管理、 远程办公连接、办公网络管理、终端安全、应用权限控制，访问审计等多项核心能力。所有企业办公应用，包括内网系统和第三方 SaaS 应用全部汇聚到一个平台，统一访问、统一管理、统一应用账号、统一内外网应用访问入口，兼容所有操作系统，统一用户体验。企业员工可以随时随地连接内部网络，高效进行远程办公。

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
![wechat](/wechat.png)


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

## 4、常见异常



