# MySQL主从复制
  使用docker部署mysql的主从复制

## 原理
  mysql的主从复制，首先是master将sql操作后的改变记录到binlog里，然后slave将binlog的事件拷贝到relay log里，slave重做relay log里的事件达到复制的目的。

## 部署
  1. 首先拉取基础镜像
  ```
  docker pull mysql
  ```
  2. 准备配置文件   
  master的配置文件：master/my.cnf
  ```
  [mysqld]
  server-id = 1 # server-id 是唯一的
  log-bin = mysql-bin # 打开日志
  ```
  slave的配置文件:slave/my.cnf
  ```
  [mysqld]
  server-id = 2
  log-bin = mysql-bin
  relay_log = mysql-relay-bin
  log_slave_updates = 1
  read_only = 1
  ```
  3. 运行容器   
    * 开启master
    ```
    docker run -d -e MYSQL_ROOT_PASSWORD=hongker --name master -v ~/Workspace/docker/mysql/master:/etc/mysql/conf.d/ mysql
    ```
    注:
      * `MYSQL_ROOT_PASSWORD`为自定义root的密码.   
      * `~/Workspace/docker/mysql/master`表示上述master的配置目录,下同(slave)

    * 开启slave
    ```
    docker run -d -e MYSQL_ROOT_PASSWORD=hongker --name slave -v ~/Workspace/docker/mysql/slave:/etc/mysql/conf.d/ mysql
    ```
  4. 配置从属关系
    * 获取master和slave的ip
  ```
  docker inspect master # 得到master的ip
  docker inspect slave # 得到slave的ip
  ```
    * 设置从属关系
    ```
    mysql -uroot -phongker -h slave_ip # 使用client连接到slave
    change master to master_host='master_ip',master_user='root',master_password='hongker'; # 设置master
    start slave; # 开启slave
    show slave status\G # 查看状态
    ```
  5. 测试
  ```
  mysql -uroot -phongker -h master_ip # 连接到master
  create database test charset=utf8;
  use test;
  create table user(id int not null,username varchar(20));
  ```
  这是去看slave里也有user这个表，表示同步成功.
