# Hello-World
![052214lunchshawshank_640x360](https://user-images.githubusercontent.com/128745328/229586010-4cd3ce23-f187-4e1b-b8dc-824e17108679.jpg)  
>**_The sun in the head, like a free man._**  
>阳光洒肩头，仿佛自由人。
## 学习强国  
### 机场
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
#### 节点搭建
```
# 更新软件源
apt update

# 启用 BBR TCP 拥塞控制算法
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p

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
