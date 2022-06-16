# Centos下安装Mysql

_Centos下默认支持的是MariaDB、目前Centos下默认支持的数据库是MariaDB,MariaDB是mysql的增强版本，由于mysql被Oracle收购之后，mysql之父担心之后mysql会变成闭源的软件，就又开发了这个版本，支持mysql的所有功能，还增加了一些mysql没有的功能，只是和mysql相比，有些操作稍微不同。**个人使用可以直接用MariaDB，省的去折腾mysql环境。**_

### 1. 安装MariaDB

#### 1.1 卸载mariaDB

```bash
查询是否安装了mariaDB
rpm -qa | grep mariadb              
如果有的话，全部删除
rpm -e --nodeps mariadb-*           #删除所有包
```

#### 1.2 安装MariaDB

```bash
一键安装MariaDB
yum install mariadb-server mariadb
```

#### 1.3 常用命令

```bash
启动服务
systemctl start mariadb      
设置开机启动              
systemctl enable mariadb     
重新启动              
systemctl restart mariadb  
停止服务               
systemctl stop mariadb.service
```

#### 1.4 简单配置

```bash
mysql -u root -p
```

初始密码为空直接跳过即可

进行MariaDB的相关简单配置,使用mysql_secure_installation命令进行配置。

```bash
mysql_secure_installation
```

进行设置密码等等。就到这里了。

### 2. yum安装Mysql

Mysql安装，由于Centos现在支持MariaDB，想要安装mysql就要先下载mysql的yum源。


#### 2.1 下载yum源

```bash
wget  http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
安装rpm包
yum -y install mysql57-community-release-el7-10.noarch.rpm
```

#### 2.2 安装Mysql服务

安装成功的话，/etc/yum.repos.d/会多两个mysql文件

```bash
ls /etc/yum.repos.d/
显示如下
mysql-community-source.repo
mysql-community.repo
```

查看mysql57的安装源是否可用，如不可用请自行修改配置文件/etc/yum.repos.d/mysql-community.repo使mysql57下面的enable=1

```bash
yum repolist enabled | grep mysql
mysql-connectors-community           MySQL Connectors Community              74
mysql-tools-community                MySQL Tools Community                   74
mysql57-community                    MySQL 5.7 Community Server             307
```

安装mysql服务

```bash
yum -y install mysql-community-server
```

#### 2.3 Mysql常用配置

启动mysql服务

```bash
systemctl start  mysqld.service
```

查看mysql运行状态

```bash
systemctl status mysqld.service
```

当看到有active (running)说明启动了。

验证mysql是否成功，与MariaDB不同的是，mysql会随机生成一个初始密码。默认在/var/log/mysqld.log里，通过以下命令可以查看初始密码，冒号后面的就是。

```bash
grep 'temporary password' /var/log/mysqld.log
```

登陆并修改密码

```bash
mysql -u root -p
```

输入刚刚得到的初始密码

修改密码策略，默认的要有大小写，字母，数字等组合才行，比较麻烦，把密码策略关掉。长度最小设为4

```bash
set global validate_password_policy=0;                #0代表密码规则关闭
set global validate_password_length=4;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';   #修改密码为root
```

Centos 下主要文件位置

```bash
/etc/my.cnf                       #这是mysql的主配置文件
/var/lib/mysql                    #mysql数据库的数据库文件存放位置
/var/log                          #mysql数据库的日志输出存放位置
```

### 3. 源码包安装Mysql

前面的安装步骤对比起来稍显简单。但是前提是需要网络环境很支持的状态下。而且yum安装的软件，目录都比较乱，需要记好多目录地址。管理起来不是很方便。源码包方式我个人比较推荐。

#### 3.1 下载源码包

下载安装包，用xftp传上到服务器上

![](https://img-blog.csdnimg.cn/20181123174517765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NjkxNzEz,size_16,color_FFFFFF,t_70#alt=%E5%9C%A8%E8%BF%99%E9%87%8C%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E6%8F%8F%E8%BF%B0)

选择的是绿色版安装时不需要网络支持。

#### 3.2 解压验证

```bash
解压到自定义目录下（/usr/local下）
tar -zxvf mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz -C /usr/local
切换到/usr/local/
cd  /usr/local
给mysql安装目录创建软链接
ln -s mysql-5.7.24-linux-glibc2.12-x86_64  mysql
给Centos添加用户组和用户（只有所有权，没有登录权限）
groupadd mysql                               #添加用户组
useradd -r -g mysql -s /bin/false mysql      #添加用户
进入到mysql目录下，把权限给新建的mysql用户
cd  mysql/                                 #切换目录
chown -R mysql:mysql ./                    #授权给mysql
```

#### 3.3 编译安装

```bash
配置安装位置
./bin/mysqld --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --initialize
```

安装好后，最后面会有个默认密码

#### 3.4 常用配置

```bash
安装目录下
./support-files/mysql.server start                  #启动服务
cp support-files/mysql.server /etc/init.d/mysqld    #把mysql进程放到service目录里
service mysqld restart   		            #测试重启服务
```

用刚刚的默认密码，登陆，修改密码

```
ln -s /usr/local/mysql/bin/mysql  /usr/bin   #创建软链接，相当于一个快捷方式，哪都可以登录
mysql -u root -p                                     #之后输入密码
alter user 'root'@'localhost' identified by 'root'; #修改密码为root
```

### 4. 设置外网访问

> 给所有ip的root用户授权，在mysql命令行下（方法一）


```bash
use mysql;
update user set user.Host='%' where user.User='root';
flush privileges;   #刷新配置
```

> 给所有root用户和使用此密码的授权，命令行下(方法二）


```bash
grant all privileges on *.* to 'root'@'%' identified by 'root' with grant option;
flush privileges;       #刷新配置
```

> 开放3306端口(Centos7下）,其他版本好像防火墙是iptables


```bash
firewall-cmd --zone=public --add-port=3306/tcp --permanent  #添加端口
firewall-cmd --reload                                      #重载
firewall-cmd --zone=public --query-port=3306/tcp           #查看
```
