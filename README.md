
# 请参与PowerX部署文档:

## [docker-compose部署](https://powerx.artisan-cloud.com/zh/v1/manual/start/installation-docker-compose.html)


# PowerX的项目

前后端分离，分别是：

PowerX：后端是独立的项目，基于Golang语言，Gozero框架开发
PowerXDahsboard：面向后台管理运营人员的Web前端项目，基于ArcoDesign框架，Vue3开发
PowerXWeb：面向客户的前端应用项目，基于ArcoDesign框架，Vue3开发


## backend

### 复制编译后的执行文件powerx和powerxctl

如果PowerX的项目，已经准备部署，那么请用make app-build-linux来进行编译，  
将项目目录下的.build/linux/下的powerx和powerxctl复制到PowerXDocker的backend目录下

### Dockerfile
编排PowerX后端的应用程序，编译成镜像

### etc
这个是属于PowerX里的配置目录，将项目相关的配置都进行了统一存放
* api_group.csv和api.csv可以通过PowerX里的cmd/ctl/powerxctl.go 
```bash
cmd/ctl/powerxctl.go  api-gen 
```
会在根目录下生成这两个文件，然后复制到PowerXDocker的backend/etc下

* minio.env 可以设置oss访问的账号密码
* mysql.cnf 预留设置
* power.yaml，最重要的配置文件
```
cp backend/etc/powerx-example.yaml backend/etc/powerx.yaml
```
然后配置后端应用的相关配置

* rbac_model.conf && rbac_policy.csv 取默认值

## dashboard
### dist
在PowerXDashboard开发完后，使用npm run build,将编译好的dist文件，复制到PowerXDocker的dashboard文件目录下

### Dockerfile
编排PowerXDahsboard后台Web的应用程序，编译成镜像

### nginx
nginx主要是为了代理项目的域名请求，以及前后端请求ip地址的设置

#### nginx.conf
这个就用默认配置即可，不用改动
#### servers && servers_ssl
这个属于nginx里的servers配置，如果没有ssl，则配置第一个即可
如果有ssl证书配置，用servers_ssl，同时将证书的目录，放在同级的ssl目录下
#### ssl
存放证书的路径，如果需要证书的话

## web
### dist
在PowerXWeb开发完后，使用npm run build,将编译好的dist文件，复制到PowerXDocker的Web文件目录下

### Dockerfile
编排PowerXWeb后台Web的应用程序，编译成镜像

### nginx
nginx主要是为了代理项目的域名请求，以及前后端请求ip地址的设置

#### nginx.conf
这个就用默认配置即可，不用改动
#### servers && servers_ssl
这个属于nginx里的servers配置，如果没有ssl，则配置第一个即可
如果有ssl证书配置，用servers_ssl，同时将证书的目录，放在同级的ssl目录下
#### ssl
存放证书的路径，如果需要证书的话


## data
data是数据目录，里面保存了数据库，和oss的数据，你可以独立备份

## gitignore
他会将log，data，等敏感数据忽略在git仓库里


## docker-compose.yaml
启动编排docker镜像

docker-compose up -d