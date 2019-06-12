## ss 一键脚本
### 1. 获取git
```shell
Centos系统执行这个： yum -y install git
Ubuntu/Debian系统执行这个： apt-get -y install git
```
### 2. 下载脚本
```shell
git clone -b master https://github.com/flyzy2005/ss-fly
ss-fly/ss-fly.sh -i password 1024 
# passowrd 是密码 1024是端口  
```

### 3. 配置文件及相关操作
```shell
修改配置文件：vim /etc/shadowsocks.json
生成ss链接：ss-fly/ss-fly.sh -sslink
停止ss服务：ssserver -c /etc/shadowsocks.json -d stop
启动ss服务：ssserver -c /etc/shadowsocks.json -d start
重启ss服务：ssserver -c /etc/shadowsocks.json -d restart
```
### 4. 卸载服务
```shell
ss-fly/ss-fly.sh -uninstall
```
### 5. bbr加速
```shell
ss-fly/ss-fly.sh -bbr
```
判断bbr加速是否成功，输入命令 `sysctl net.ipv4.tcp_available_congestion_control`
如果返回值为 `net.ipv4.tcp_available_congestion_control = bbr cubic reno` 。只要带有bbr即可
