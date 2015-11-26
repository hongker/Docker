#Registry
  搭建本地仓库

##创建仓库
  将仓库地址设为 localhost:5000    
  ```
  docker run -d -p 5000:5000 --restart=always --name registry registry:2
  ```
##提交镜像
  给一个本地已有的镜像(ubuntu)设定标签
  ```
  docker tag ubuntu localhost:5000/ubuntu
  ```

  提交
  ```
  docker push ubuntu localhost:5000/ubuntu
  ```

##从本地仓库下载镜像
  ```
  docker pull ubuntu localhost:5000/ubuntu
  ```

##删除仓库
  ```
  docker stop registry && docker rm -v registry
  ```
