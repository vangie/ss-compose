#  SSCompose

使用 docker-compose 整合了最新的 [shadowsocksr 多用户版本](https://github.com/shadowsocksr/shadowsocksr)和 [ss-panelv3](https://github.com/orvice/ss-panel)，并默认开启了 auth_aes128_sha1 协议和 tls1.2_ticket_auth 混淆。

## 安装 docker 和 docker-compose

以下是在 ubuntu 16.04 系统上安装 docker 和 docker-compose，其他系统请自行调整。

#### 安装 docker
```
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 \
    --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
sudo apt-add-repository 'deb https://apt.dockerproject.org/repo ubuntu-xenial main'
sudo apt-get update
sudo apt-get install -y docker-engine
```

#### 安装 docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.11.2/docker-compose-$(uname -s)-$(uname -m)" \
    -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

#### 将当前用户加入 docker 组，免 sudo 执行 docker 命令

```
sudo usermod -aG docker $(whoami)
```

## 克隆项目

```
git clone https://github.com/vangie/ss-compose.git
```

## 构建 image 并运行 container

```
cd ss-compose
# 将下面 URL 地址改成你的实际地址
BASE_URL=http://yourdomain.com docker-compose up -d
```
## 添加管理员账户

```
root@hk-01:~/ss-compose# docker exec -it sscompose_php_1 sh
/var/www/html # php xcat createAdmin
```

## 添加节点
用刚才创建的管理员账户登陆系统，然后进入【管理员面板】，然后选择【节点管理】菜单，添加一个新节点

* 节点名称： 1
* 节点地址： 你的域名或者 IP 地址
* 加密方式： aes-256-cfb

## 客户端配置

shadowsocksr 是兼容老的 ss 客户端协议，但是老的客户端无法启用[协议特性和混淆特性](https://github.com/breakwa11/shadowsocks-rss/blob/master/ssr.md)以达到最佳的抗掉包效果。所以推荐使用如下客户端

* [Windows] / [macOS]
* [Android] / [iOS]
* [OpenWRT]

客户端配置

* 协议(protocol)： auth_aes128_sha1
* 混淆(obfs)：     tls1.2_ticket_auth


[OpenWRT]:           https://github.com/shadowsocks/openwrt-shadowsocks
[macOS]:             https://github.com/shadowsocksr/ShadowsocksX-NG
[Windows]:           https://github.com/shadowsocksr/shadowsocksr-csharp
[iOS]:               https://github.com/shadowsocks/shadowsocks-iOS/wiki/Help
[Android]:           https://github.com/shadowsocksr/shadowsocksr-android
