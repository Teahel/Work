
### 安装nginx
#### 1、下载安装包，解压
可以选定版本 当前版本为20
```
wget http://nginx.org/download/nginx-1.20.1.tar.gz  && tar xf nginx-1.20.1.tar.gz -C /opt/project
```
#### 2、要使用tcp分流需要用到stream 包，初始化配置的时候
需要到 包含configure文件夹下 
```
./configure --prefix=/data/nginx --with-http_gunzip_module --with-http_ssl_module  --with-stream 
```
- /data/nginx: nginx.conf 配置文件地方
- --with-stream  tcp 分流
- --with-http_gunzip_module --with-http_ssl_module http需求

#### 3、编译和安装
需要到 包含configure文件夹下 
```
make && make install
```
#### 4、服务配置
```
cd /lib/systemd/system/
vi nginx.service
```
创建文件，内容如下

```
[Unit]
Description=nginx 
After=network.target 
   
[Service] 
Type=forking 
ExecStart=/opt/project/nginx-1.20.1/objs/nginx
ExecReload=/opt/project/nginx-1.20.1/objs/nginx reload
ExecStop=/opt/project/nginx-1.20.1/objs/nginx quit
PrivateTmp=true 
   
[Install] 
WantedBy=multi-user.target
```
启动文件
```
systemctl enable nginx.service
```

```
systemctl start nginx.service    启动nginx

systemctl stop nginx.service    结束nginx

systemctl restart nginx.service    重启nginx
```

下方命令可以知道nginx.conf 是否配置成功以及部署位置

```
./nginx -t
```
