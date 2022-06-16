
- [前言](#%E5%89%8D%E8%A8%80)
- [环境](#%E7%8E%AF%E5%A2%83)
- [安装git](#%E5%AE%89%E8%A3%85git)
- [创建登录证书](#%E5%88%9B%E5%BB%BA%E7%99%BB%E5%BD%95%E8%AF%81%E4%B9%A6)
- [创建git用户和用户组](#%E5%88%9B%E5%BB%BAgit%E7%94%A8%E6%88%B7%E5%92%8C%E7%94%A8%E6%88%B7%E7%BB%84)
   - [创建密钥仓库](#%E5%88%9B%E5%BB%BA%E5%AF%86%E9%92%A5%E4%BB%93%E5%BA%93)
   - [初始化仓库](#%E5%88%9D%E5%A7%8B%E5%8C%96%E4%BB%93%E5%BA%93)
   - [克隆仓库测试](#%E5%85%8B%E9%9A%86%E4%BB%93%E5%BA%93%E6%B5%8B%E8%AF%95)
   - [一些错误处理](#%E4%B8%80%E4%BA%9B%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)

### 前言

> 我们平时git拉取代码，拉取项目都是储存在托管平台的，我们可以使用git的来创建属于自己的git服务器仓库，下面记录一下学习过程。


### 环境

_阿里云Centos7.3_

_git 2.8.3_

### 安装git

[源码包安装git](https://blog.csdn.net/qq_34691713/article/details/84987340)

### 创建登录证书

> 也就是存储使用者git公钥的地方


#### 创建git用户和用户组

```
groupadd git
useradd git -g git
```

#### 创建密钥仓库

```
cd /home/git    
mkdir .ssh
chmod 755 .ssh
touch .ssh/authorized_keys          #git公钥仓库
chmod 644 .ssh/authorized_keys
```

#### 初始化仓库

> 创建一个仓库地址，我选择的是/home/githome/下


```
cd /home
mkdir githome
chown git:git githome/  #权限给git组下的git用户
cd githome
git init --bare demo.git  #初始化一个仓库
chown -R git:git demo.git #仓库所属用户改成git
```

#### 克隆仓库测试

> 把自己电脑上的git密钥添加创建的authorized_keys里


```
git clone git@47.106.113.227:/home/githome/demo.git
正常情况下，会出现以下
Cloning into 'demo'...
warning: You appear to have cloned an empty repository.
```

#### 一些错误处理

```
bash: git-upload-pack: command not found
```

原因是源码包安装的git，没有把/usr/local/git/bin下的一些常用启动项添加/usr/bin下，找不到命令

解决办法

```
ln -s /usr/local/git/bin/git-upload-pack  /usr/bin
```

同样的问题，上传到库的时候，出现

```
bash: git-receive-pack: command not found
```

解决方法如上
