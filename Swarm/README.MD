#Swarm
  使用Swarm管理Docker集群。


##安装
  docker pull swarm

##修改Docker服务监听端口  
  vim /etc/default/docker     
  在文件最后添加一句：    
    D0OCKER_OPTS="-H 0.0.0.0:4243 –H unix:///var/run/docker.sock"

##重启docker    
    service docker restart     

##创建集群标识
  docker run --rm swarm create
  返回一个唯一Token: cefdba66e7828d2c1e9d7205374b3686      


##根据Token加入集群
  docker run --rm   swarm join --addr=192.168.0.105:4243 token://cefdba66e7828d2c1e9d7205374b3686    

## 启动Swarm Manager
  docker run -d -p 5555:4243  swarm manage token://cefdba66e7828d2c1e9d7205374b3686    
  使用5555端口映射docker端口    

## 列表查看Swarm管理的容器
  docker run --rm swarm list token://cefdba66e7828d2c1e9d7205374b3686    
