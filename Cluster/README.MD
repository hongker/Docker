##创建集群
  使用Docker来搭建一个由容器构成的简单的Web集群。

##集群架构
  Web服务器(2台),Mysql服务器(2台),负载器(1台)    
  Web服务器采用 nginx+php-fpm,负载器使用Nginx    

##Dockerfile
  Web服务器
  ```
  #THis is dockerfile of my web server
  FROM ubuntu
  MAINTAINER Docker Hongker<xiaok2013@live.com>

  #国内用户如果嫌更新网速慢可以先更换源
  ```
