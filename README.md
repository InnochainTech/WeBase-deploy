# WeBase-deploy
webase 一键部署文件

## 前提条件

|  环境   | 版本  |
|  ----  | ----  |
|  Java  | JDK 8 至JDK 14  |
| MySQL  | MySQL-5.6及以上 |
| Python  | Python3.6及以上 |
| PyMySQL  |  |

## 拉取部署脚本
获取部署安装包：
```shell
wget https://github.com/InnochainTech/WeBase-deploy/releases/download/v1.6.0/webase-deploy.zip
```
解压安装包：

```shell
unzip webase-deploy.zip
```

进入目录：
```shell
cd webase-deploy
```

## 修改配置

1. 修改配置文件（vi common.properties）；
2. 一键部署支持使用已有链或者搭建新链。通过参数”if.exist.fisco”配置是否使用已有链，以下配置二选一即可：
 - 当配置”yes”时，需配置已有链的路径fisco.dir。路径下要存在sdk目录，sdk目录中包含ca.crt, sdk.crt, sdk.key及gm目录，gm目录中包含国密SSL所需证书，包含gmca.crt、gmsdk.crt、gmsdk.key、gmensdk.crt和gmensdk.key
 - 当配置”no”时，需配置节点fisco版本和节点安装个数，搭建的新链默认两个群组
3. 服务端口不能小于1024
4. 部署时，修改 common.properties 配置文件

```properties
# WeBASE子系统的最新版本(v1.1.0或以上版本)
webase.web.version=v1.5.3
webase.mgr.version=v1.5.3
webase.sign.version=v1.6.0
webase.front.version=v1.5.3

#####################################################################
## 使用Docker启用Mysql服务，则需要配置以下值

# 1: enable mysql in docker
# 0: mysql run in host, required fill in the configuration of webase-node-mgr and webase-sign
docker.mysql=1

# if [docker.mysql=1], mysql run in host (only works in [installDockerAll])
# run mysql 5.6 by docker
docker.mysql.port=23306
# default user [root]
docker.mysql.password=123456

#####################################################################
## 不使用Docker启动Mysql，则需要配置以下值

# 节点管理子系统mysql数据库配置
mysql.ip=127.0.0.1
mysql.port=3306
mysql.user=dbUsername
mysql.password=dbPassword
mysql.database=webasenodemanager

# 签名服务子系统mysql数据库配置
sign.mysql.ip=localhost
sign.mysql.port=3306
sign.mysql.user=dbUsername
sign.mysql.password=dbPassword
sign.mysql.database=webasesign



# 节点前置子系统h2数据库名和所属机构
front.h2.name=webasefront
front.org=fisco

# WeBASE管理平台服务端口
web.port=5000
# 启用移动端管理平台 (0: disable, 1: enable)
web.h5.enable=1

# 节点管理子系统服务端口
mgr.port=5001
# 节点前置子系统端口
front.port=5002
# 签名服务子系统端口
sign.port=5004


# 节点监听Ip
node.listenIp=127.0.0.1
# 节点p2p端口
node.p2pPort=30300
# 节点链上链下端口
node.channelPort=20200
# 节点rpc端口
node.rpcPort=8545

# 加密类型 (0: ECDSA算法, 1: 国密算法)
encrypt.type=0
# SSL连接加密类型 (0: ECDSA SSL, 1: 国密SSL)
# 只有国密链才能使用国密SSL
encrypt.sslType=0

# 是否使用已有的链（yes/no）
if.exist.fisco=no

# 使用已有链时需配置
# 已有链的路径，start_all.sh脚本所在路径
# 路径下要存在sdk目录（sdk目录中包含了SSL所需的证书，即ca.crt、sdk.crt、sdk.key和gm目录（包含国密SSL证书，gmca.crt、gmsdk.crt、gmsdk.key、gmensdk.crt和gmensdk.key）
fisco.dir=/data/app/nodes/127.0.0.1
# 前置所连接节点，在127.0.0.1目录中的节点中的一个
# 节点路径下要存在conf文件夹，conf里存放节点证书（ca.crt、node.crt和node.key）
node.dir=node0

# 搭建新链时需配置
# FISCO-BCOS版本
fisco.version=2.7.2
# 搭建节点个数（默认两个）
node.counts=nodeCounts
```

## 部署
- 执行installAll命令，部署服务将自动部署FISCO BCOS节点，并部署 WeBASE 中间件服务，包括签名服务（sign）、节点前置（front）、节点管理服务（node-mgr）、节点管理前端（web）

```shell
# 部署并启动所有服务
python3 deploy.py installAll
```

更多请详见[ficos_bcos 部署文档]("https://webasedoc.readthedocs.io/zh_CN/latest/docs/WeBASE/install.html")
