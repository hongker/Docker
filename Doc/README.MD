#Doc
  本地安装Docker官方文档

##安装
  1.先从Github上把docker克隆下来    
  ```
    git clone https://github.com/docker/docker.git
  ```

  2.进入文档所在目录    
  ```
    cd ./docker
  ```

  3.将DOCKER_HOST存入.barhrc里
  export DOCKER_HOST=tcp://localhost:4243

  4.make docs
