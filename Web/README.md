## 使用Docker搭建Web服务器

### 1.Nginx + php-fpm
  (1) 编写Dockerfile

  ```
   FROM phusion/baseimage
   MAINTAINER hongker "xiaok2013@qq.com"
   RUN apt-get update -y
   RUN apt-get install -y nginx php5-fpm wget

   RUN wget -O /etc/nginx/sites-available/default https://gist.github.com/darron/6159214/raw/30a60885df6f677bfe6f2ff46078629a8913d0bc/gistfile1.txt

   RUN echo "cgi.fix_pathinfo = 0;" >> /etc/php5/fpm/php.ini
   RUN echo "daemon off;" >> /etc/nginx/nginx.conf
   RUN mkdir /var/www
   RUN echo "<?php phpinfo(); ?>" > /var/www/index.php

   EXPOSE 80

   CMD service php5-fpm start && nginx

  ```
  (2)创建镜像
  ```
  docker build -t web/nginx:dockerfile .
  ```

  (3)运行容器
  ```
  docker run -d -name=web_nginx_node_1 web/nginx:dockerfile
  ```

  (4)查看IP
  docker inspect web_nginx_node_1 |grep IPA


### 2.Apache2 + php5
  (1) 编写Dockerfile

  ```
  FROM phusion/baseimage
  MAINTAINER hongker "xiaok2013@qq.com"
  RUN apt-get update -y
  RUN apt-get install -y apache2
  RUN apt-get install -y php5


  RUN echo "<?php phpinfo(); ?>" > /var/www/html/index.php

  EXPOSE 80

  CMD service apache2 start

  ```
  (2)创建镜像
  ```
  docker build -t web/apache2:dockerfile .
  ```

  (3)运行容器
  ```
  docker run -d -name=web_apache2_node_1 web/apache2:dockerfile
  ```

  (4)查看IP
  docker inspect web_apache2_node_1 |grep IPA

### 3.性能测试
  (1)对nginx+php-fpm ： ab -c 100 -n 10000 http://172.17.0.2/

  ```
  This is ApacheBench, Version 2.3 <$Revision: 1638069 $>
  Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
  Licensed to The Apache Software Foundation, http://www.apache.org/

  Benchmarking 172.17.0.2 (be patient)
  Completed 1000 requests
  Completed 2000 requests
  Completed 3000 requests
  Completed 4000 requests
  Completed 5000 requests
  Completed 6000 requests
  Completed 7000 requests
  Completed 8000 requests
  Completed 9000 requests
  Completed 10000 requests
  Finished 10000 requests


  Server Software:        nginx/1.4.6
  Server Hostname:        172.17.0.2
  Server Port:            80

  Document Path:          /
  Document Length:        11 bytes

  Concurrency Level:      100
  Time taken for tests:   1.397 seconds
  Complete requests:      10000
  Failed requests:        0
  Total transferred:      1780000 bytes
  HTML transferred:       110000 bytes
  Requests per second:    7158.25 [#/sec] (mean)
  Time per request:       13.970 [ms] (mean)
  Time per request:       0.140 [ms] (mean, across all concurrent requests)
  Transfer rate:          1244.30 [Kbytes/sec] received

  Connection Times (ms)
  min  mean[+/-sd] median   max
  Connect:        0    0   0.3      0       4
  Processing:     3   14   2.8     13      25
  Waiting:        3   14   2.8     13      25
  Total:          7   14   2.9     13      27

  Percentage of the requests served within a certain time (ms)
  50%     13
  66%     14
  75%     16
  80%     16
  90%     19
  95%     19
  98%     20
  99%     21
  100%     27 (longest request)

  ```

  (2)对apache2+php5 ： ab -c 100 -n 10000 http://172.17.0.3/
  ```
  This is ApacheBench, Version 2.3 <$Revision: 1638069 $>
  Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
  Licensed to The Apache Software Foundation, http://www.apache.org/

  Benchmarking 172.17.0.3 (be patient)
  Completed 1000 requests
  Completed 2000 requests
  Completed 3000 requests
  Completed 4000 requests
  Completed 5000 requests
  Completed 6000 requests
  Completed 7000 requests
  Completed 8000 requests
  Completed 9000 requests
  Completed 10000 requests
  Finished 10000 requests


  Server Software:        Apache/2.4.7
  Server Hostname:        172.17.0.3
  Server Port:            80

  Document Path:          /
  Document Length:        12 bytes

  Concurrency Level:      100
  Time taken for tests:   2.135 seconds
  Complete requests:      10000
  Failed requests:        0
  Total transferred:      2000000 bytes
  HTML transferred:       120000 bytes
  Requests per second:    4682.75 [#/sec] (mean)
  Time per request:       21.355 [ms] (mean)
  Time per request:       0.214 [ms] (mean, across all concurrent requests)
  Transfer rate:          914.60 [Kbytes/sec] received

  Connection Times (ms)
  min  mean[+/-sd] median   max
  Connect:        0    0   0.2      0       2
  Processing:     2   21   4.2     20      39
  Waiting:        2   21   4.2     19      39
  Total:          4   21   4.2     20      39

  Percentage of the requests served within a certain time (ms)
  50%     20
  66%     22
  75%     24
  80%     25
  90%     27
  95%     29
  98%     32
  99%     34
  100%     39 (longest request)

  ```

  (3)最终结果:
  ```
  Nginx : Requests per second:    7158.25 [#/sec] (mean)

  Apache : Requests per second:    4682.75 [#/sec] (mean)
  ```
  <br />
  PS : 果然Nginx并发更强一点啊...
