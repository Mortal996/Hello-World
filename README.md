# Hello-World
![052214lunchshawshank_640x360](https://user-images.githubusercontent.com/128745328/229586010-4cd3ce23-f187-4e1b-b8dc-824e17108679.jpg)  
>**_The sun in the head, like a free man._**  
>阳光洒肩头，仿佛自由人。
## 学习强国  

### 自建

#### 准备
+ 购买vps
  + [挖站否](https://wzfou.com/vps-bangdan)
  + [Vultr](https://www.vultr.com)
  + [CloudCone](https://app.cloudcone.com/?ref=9510)
  + [AWS](https://aws.amazon.com)
  + [搬瓦工库存](https://teddysun.com/bwh.html)
+ 购买域名  
  + [Namesilo](https://www.namesilo.com/?rid=a5e2f22jo)  
  + [GoDaddy](https://www.godaddy.com/)
+ 域名托管
  + [Cloudflare](https://dash.cloudflare.com)  
+ SSH连接
  + [FinalShell](http://www.hostbuf.com/t/988.html)
+ 测速ping
  + [speedtest](https://www.speedtest.net)
  + [fast](https://fast.com)
  + [站长之家](https://ping.chinaz.com)
  + [whoer](https://whoer.net/zh)
  + [IP/DNS](https://ipleak.net)
  + [My IP](https://browserleaks.com/ip)

#### [DD系统](https://blog.laoda.de/archives/dd)
+ 准备工作
```
# Debian 与 Ubuntu
apt-get install -y xz-utils openssl gawk file
```
+ 一键脚本
```
wget --no-check-certificate -O AutoReinstall.sh https://git.io/AutoReinstall.sh && bash AutoReinstall.sh
```
>密码：Centos默认密码`Pwd@CentOS`，其它系统`Pwd@Linux`
+ 输入`cat /etc/os-release`查看版本
+ 常用的命令
```
apt install sudo curl wget 
```
+ 更新安装包索引
```
apt update 

apt upgrade 
```
+ Debian 系统比较干净，安装常用的软件
```
apt install wget curl sudo vim git -y
```

#### [开启 TCP BBR 拥塞控制算法](https://blog.laoda.de/archives/hello-docker#5%E5%AE%89%E8%A3%85dockerdocker-compose)  
新的 TCP 拥塞控制算法 BBR (Bottleneck Bandwidth and RTT) 可以让服务器的带宽尽量跑满，并且尽量不要有排队的情况，让网络服务更佳稳定和高效。

Linux Kernel 内核升级到 4.9 及以上版本可以实现 BBR 加速（Ubuntu 18.04 默认的内核是 4.15版本的内核，Ubuntu 20.04 默认的内核是 5.4 版本的内核，并已经默认编译了 TCP BBR 模块，可以直接通过参数开启）。

目前 De­bian 10 自带的内核版本一般是4.19，我们可以查看一下内核版本（如果是 5.6 及以上内核则集成了 Wire­Guard ）  
`hostnamectl`   #或者 `uname -r`查看内核版本  

1.给 Debian 10 添加官方 backports 源，获取更新的软件库
```
sudo nano /etc/apt/sources.list
```
2.然后把下面这一条加在最后，并保存退出。
```
deb http://deb.debian.org/debian buster-backports main
```
3.刷新软件库并查询 Debian 官方的最新版内核并安装。请务必安装你的 VPS 对应的版本（本文以比较常见的【amd64】为例）。
```
sudo apt update && sudo apt -t buster-backports install linux-image-amd64
```
4.修改 kernel 参数配置文件 sysctl.conf 并指定开启 BBR
```
sudo nano /etc/sysctl.conf
```
5.把下面的内容添加进去
```
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
```
6.重启 VPS、使内核更新和BBR设置都生效
```
sudo reboot
```
如果你想确认 BBR 是否正确开启，可以使用下面的命令：
```
lsmod | grep bbr
```
如果你想确认 fq 算法是否正确开启，可以使用下面的命令：
```
sch_fq
```
>一键开启BBR
>注意：需要Linux Kernel 内核升级到 4.9 及以上版本可以实现 BBR 加速
```
uname -srm

Linux 3.10.0-957.12.2.el7.x86_64 x86_64
3 - 内核版本.
10 - 主修订版本.
0-957 - 次要修订版本.
12 - 补丁版本.
```
>Ubuntu18.04以上就可以(默认的内核4.15)
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

sysctl -p

sysctl net.ipv4.tcp_available_congestion_control

lsmod | grep bbr
```

#### 修改时间
```
apt update -y && apt upgrade -y  #更新一下包
```
查看时间：
```
timedatectl
```
```
timedatectl list-timezones #列出所有时区
```
```
sudo timedatectl set-timezone Asia/Hong_Kong #改成香港
```

#### 添加SWAP
swap是Linux中的虚拟内存，用于扩充物理内存不足而用来存储临时数据存在的。它类似于Windows中的虚拟内存。在Windows中，只可以使用文件来当作虚拟内存。而linux可以文件或者分区来当作虚拟内存。

这个虚拟内存对于内存小的VPS非常有必要，可以提高我们的运行效率。
```
wget -O box.sh https://raw.githubusercontent.com/BlueSkyXN/SKY-BOX/main/box.sh && chmod +x box.sh && clear && ./box.sh
```
18

#### 安装Docker、Docker-compose
+ 更新、安装必备软件
```
apt-get update && apt-get install -y wget vim
```
+ 非大陆Docker安装
```
wget -qO- get.docker.com | bash
```
+ 查看Docker版本
```
docker -v
```
+ 开机自动启动
```
systemctl enable docker
```
+ 卸载Docker
```
sudo apt-get purge docker-ce docker-ce-cli containerd.io

sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
+ 非大陆Docker-compose安装
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
+ 修改Docker配置
```
cat > /etc/docker/daemon.json <<EOF
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
EOF
```
+ 重启 Docker 服务
```
systemctl restart docker
```

#### 整理一些常用的脚本
+ 查看 Linux 系统信息，还可以测试网络带宽及硬盘读写速率
```
wget -qO- bench.sh | bash
```
+ 检测是否可以访问ChatGPT脚本
```
bash <(curl -Ls https://cpp.li/openai)
```
+ 杜甫测试
```
wget -q https://github.com/Aniverse/A/raw/i/a && bash a
```
+ 显示延迟、抖动
```
bash <(wget -qO- https://bench.im/hyperspeed)
```
+ 显示回程线路
```
curl https://raw.githubusercontent.com/zhucaidan/mtr_trace/main/mtr_trace.sh|bash

wget -q route.f2k.pub -O route && bash route

wget -qO- git.io/besttrace | bash
```
+ 带综合测试的版本（CPU信息、Geekbench等）
```
bash <(wget --no-check-certificate -O- https://dl.233.mba/d/sh/superbenchpro.sh)
```
+ 测试IPv4优先还是IPv6优先
```
curl ip.p3terx.com
```
+ 全媒体测试
```
bash <(curl -L -s https://raw.githubusercontent.com/lmc999/RegionRestrictionCheck/main/check.sh)
```
+ 奈飞测试
```
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/2.5/nf_2.5_linux_amd64 && chmod +x nf && clear && ./nf
```
+ GCP VPS主机支持Root的密码登录
```
#切换到root角色
sudo -i 
#修改SSH配置文件/etc/ssh/sshd_config
vi /etc/ssh/sshd_config
#修改PermitRootLogin和PasswordAuthentication为yes
# Authentication:
PermitRootLogin yes //默认为no，需要开启root用户访问改为yes
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes //默认为no，改为yes开启密码登陆
#给root用户设置密码
passwd root
#重启SSH服务使修改生效
/etc/init.d/ssh restart
#或者是：
service sshd restart
```
#### 节点搭建
```
# 更新软件源
apt update

# 安装x-ui：
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)

# 安装nginx
apt install nginx

# 安装acme：
curl https://get.acme.sh | sh

# 添加软链接：
ln -s  /root/.acme.sh/acme.sh /usr/local/bin/acme.sh
# 切换CA机构： 
acme.sh --set-default-ca --server letsencrypt

# 申请证书： 
acme.sh  --issue -d 域名 -k ec-256 --webroot  /var/www/html

# 安装证书：
acme.sh --install-cert -d 域名 --ecc --key-file /etc/x-ui/server.key  --fullchain-file /etc/x-ui/server.crt --reloadcmd "systemctl force-reload nginx"
```
+ 配置nginx
配置文件路径：/etc/nginx/nginx.conf
```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 1024;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    gzip on;

    server {
        listen 443 ssl;
        
        server_name 12345.com;  #你的域名
        ssl_certificate       /etc/x-ui/server.crt;  #证书位置
        ssl_certificate_key   /etc/x-ui/server.key; #私钥位置
        
        ssl_session_timeout 1d;
        ssl_session_cache shared:MozSSL:10m;
        ssl_session_tickets off;
        ssl_protocols    TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers off;

        location / {
            proxy_pass https://www.huya.com; #伪装网址
            proxy_redirect off;
            proxy_ssl_server_name on;
            sub_filter_once off;
            sub_filter "www.huya.com" $server_name;
            proxy_set_header Host "www.huya.com";
            proxy_set_header Referer $http_referer;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header User-Agent $http_user_agent;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header Accept-Encoding "";
            proxy_set_header Accept-Language "zh-CN";
        }


        location /df253d5c-82f2-442e-c73a-dd454b77c199 {   #分流路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:10086; #Xray端口
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        
        location /df253d5c-82f2-442e-c73a-dd454b77c199-xui {   #xui路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:9999;  #xui监听端口
            proxy_http_version 1.1;
            proxy_set_header Host $host;
        }
    }

    server {
        listen 80;
        location /.well-known/ {
               root /var/www/html;
            }
        location / {
                rewrite ^(.*)$ https://$host$1 permanent;
            }
    }
}
```
> 每次修改nginx配置文件后必须使用 `systemctl reload nginx` 命令重新加载配置文件
#### Cloudflare Warp 原生 IP
+ [脚本链接](https://github.com/P3TERX/warp.sh)  
+ 一键脚本  
```
bash <(curl -fsSL git.io/warp.sh) [SUBCOMMAND]
```
+ WARP 脚本功能菜单  
`bash <(curl -fsSL git.io/warp.sh) menu`
+ 先后执行如下安装：  
1 - 安装 Cloudflare WARP 官方客户端  
4 - 安装 WireGuard 相关组件  
7 - 自动配置 WARP WireGuard 双栈全局网络  
+ 使用  `curl ipinfo.io`命令来检查你的 IP 地址，如果显示的是 Cloudflare 的 IP 地址，那么恭喜你，你已经成功了。  
#### NetFlix解锁检测脚本
+ [链接](https://github.com/sjlleo/netflix-verify.git)  
+ 一键脚本
```
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/v3.1.0/nf_linux_amd64 && chmod +x nf && ./nf
```
## 备份
+ [科学上网|酷壳](https://haoel.github.io/)
+ [数字移民](https://blog.shuziyimin.org/)
+ [P3TERX ZONE](https://p3terx.com/archives/cheap-and-costeffective-vps-recommended.html)
+ [我不是咕咕鸡](https://blog.laoda.de)
+ [自由上网](https://github.com/Alvin9999/new-pac)
+ [粉碎棱镜（PRISM Break）](https://prism-break.org/zh-CN/)
