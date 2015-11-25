# Docker
  Study Docker

## Docker安装
  ubuntu下：    
  ```
  sudo add-apt-repository ppa:dotcloud/lxc-docker     
  sudo update     
  sudo install lxc-docker    
  ```

  启动服务    

  ```
  service docker start
  ```

## Docker名词解析
  1.镜像：用来创建容器的只读模板。    
  2.容器：由镜像创建的运行实例。  
  3.仓库：集中存放镜像的地方。   

## Docker常用命令
  搜索镜像
  ```
  docker search ubuntu
  ```
  下载镜像
  ```
  docker pull ubuntu
  ```
  列出本地所有镜像
  ```
  docker images
  ```
  删除镜像
  ```
  docker rmi <image id>
  ```

  创建容器
  ```
  docker run -t -i -p port:port image command
  ```
  列出所有运行中容器
  ```
  docker ps -a
  ```
  删除容器
  ```
  docker rm containerId
  ```
  启动、停止和重启一个或多个指定容器
  ```
  docker start|stop|restart
  ```
  导出容器
  ```
  docker export containerId > container.tar
  ```
  导入容器快照为镜像
  ```
  cat container.rar | docker import - ubuntu/apache:1
  ```
##Docker示例
1.[使用Docker搭建Web集群服务](https://github.com/hongker/Docker/blob/master/Cluster)   
2.[安装Shipyard管理容器](https://github.com/hongker/Docker/blob/master/Shipyard)    
3.[使用Docker安装Redis集群](https://github.com/hongker/Docker/blob/master/Redis)       

##Todo
1.[使用Docker搭建即时通讯服务](#)    
2.[使用Docker搭建WebService服务](#)    
3.这货还能干啥呢....    
