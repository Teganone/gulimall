# gulimall

### docker

docker hup

### docker安装mysql

![image-20231130085918567](/Users/tegan/Library/typora/gulimall-images/image-20231130085918567.png)

 

```bash 
docker run -d -p 3306:3306 --name mysql01 -v ./mysql/config/my.conf:/etc/my.cof -v ./mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root123456  --platform linux/amd64 mysql:latest
5c4b12f2fd73bba7739aa0913b9f2e172ec85f66bfcfbb42ab134d7f06bbf492
```

> -d 后台运行容器
>
> -p 3306:3306 指定端口映射（主机端口：容器端口）
>
> --name  为容器制定一个名称
>
> --e 设置环境变量
>
> -v 挂载：
>
> * -v ./mysql/config/my.conf:/etc/my.cof 映射配置文件
>
> * -v ./mysql/data:/var/lib/mysql 映射数据目录
>
> * 其他映射方式： -v ./mysql/log:/var/log/mysql -v ./mysql/data:/var/lib/mysql -v ./mysql/config:/etc/mysql 
>
>  mysql:8.0.31 镜像名称和版本号
>
> --platform linux/amd64  解决冲突：WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested

容器运行之后，实际上可以把容器看成一台独立完整的Linux主机，可以通过:

```bash
docker exec -it 5c4 /bin/bash
```

进入容器内部查看：

```bash
(base) tegan@TegandeMacBook-Air docker % docker exec -it 5c4 /bin/bash
root@5c4b12f2fd73:/# whereis mysql
mysql: /usr/bin/mysql /usr/lib/mysql /etc/mys ql
root@5c4b12f2fd73:/# cd /var/log
root@5c4b12f2fd73:/var/log# ls
alternatives.log  apt  btmp  dpkg.log  faillog	lastlog  wtmp
root@5c4b12f2fd73:/var/log# exit
exit
```

因此创建容器的时候，可以通过 `-v` 实现对容器内部的关键文件（eg：config、log）挂载到当前主机下，方便在主机内修改、查看容器内部的变化。——相当于**快捷方式**。

配置mysql的编码: mysql/conf/my.conf:

```txt
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
init_connect='SET collation_connection =   utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```



### docker 安装 redis

docker exec -it redid regis-cli 

持久化存储。：在redis.conf中设置：

```txt
appendonly yes
```

可视化界面：redis desktop manager



### 开发环境

maven的配置文件：/config/setting.xml

idea能add as maven project





### 数据库设计

电商系统数据量很大，外键关联非常耗费数据库性能（每次插入/查询都要对外键进行检查：数据的一致性和完整性）——不建立外键。

后台管理系统用renren





前端：

前端开发，少不了 node,js; Node,js 是一个基于 ChromeV8 引擎的 JavaScript 运行环境,http://nodejs.cn/api/
我们关注于 node.js的npm 功能就行;
NPM 是随同 NodeJS一起安装的**包管理工具**，**JavaScript-NPM，Java-Maven;**

1. 官网下载安装node.js，并使用 node-v 检查版本) ；

2. 配置npm 使用淘宝镜像

  ```bash
  npm config set registry http://registry.npm.taobao.org/
  ```

  

npm如何管理包依赖：元文件 `package.json` ,类似maven的`pom.xml`.
