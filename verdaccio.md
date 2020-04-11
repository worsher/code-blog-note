# 使用Verdaccio搭建npm私服

### 干这事儿的起因
1. 最近被自定义组件困扰，想将第三方的组件库，比如elementUI落地自定制化
2. 忽然看到一个介绍npm私服搭建的公众号文章，觉得verdaccio入门的门槛比较低，搭建特别方便
3. 最近有了一个自己的云服务器，手痒难耐

### 搭建的过程
其实搭建的过程很简单，网上的教程或者npm官方的包说明也很详细（[官方地址](https://www.npmjs.com/package/verdaccio)）
命令如下：

`npm install --global verdaccio`

然后使用简单命令启动就行

`verdaccio`

此时如果是在本地直接访问http://localhost:4873/即可，但是到了服务器上就无法访问，这其中就有几个坑：
1. verdaccio本身只提供本地访问，无法使用IP进行访问，因此将localhost替换为服务器外网IP时就无法访问
2. 为了解决1，根据网上的方案，安装nginx进行访问代理，但是使用yum安装完无法查找到nginx.conf文件

解决以上两个问题的方案：
1. verdaccio使用IP进行访问的两个方案
（1）使用nginx进行代理访问，添加以下代码，已知会出现问题2（已解决）
```
server {
  listen       80;
  server_name  registry.npm.your.server;
  location / {
    proxy_pass    http://127.0.0.1:4873/;
    proxy_set_header        Host $host;
  }
}server {
  listen       80;
  server_name  registry.npm.your.server;
  location / {
    proxy_pass    http://127.0.0.1:4873/;
    proxy_set_header        Host $host;
  }
}
```
此处必须通过80端口代理，若使用非80端口代理，会出现部分JS文件404的问题，经测试为部分JS文件未通过当前url访问，而是直接使用了域名，这个问题还有待解决。
（2）在verdaccio配置文件（文件地址/root/.config/verdaccio/config.yaml）的末尾添加以下代码即可使用IP访问
`listen: 0.0.0.0:4873`

2. yum安装的nginx没有配置文件的问题
将yum的nginx源配置替换为官方的源即可，缺点就是之后如果再次安装无法更新新版本
（1）安装前置包
`sudo yum install yum-utils`
（2）替换/etc/yum.repos.d/nginx.repo文件为以下内容
```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```
（3）再次执行命令安装即可
`yum install nginx`