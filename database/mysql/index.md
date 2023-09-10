# MySQL 压缩包版 下载 安装与配置 (win10版实验)

[参考链接](https://blog.csdn.net/qq_57427605/article/details/128856984)

## 下载
[mysql download](https://downloads.mysql.com/archives/community/)

## 解压到想要的位置
一般是解压到空间较大的磁盘，比如解压到D:\\Program-Files\\mysql-8.0.32-winx64文件夹

## 配置
在解压的根文件夹里新建 my.ini 文件
```
[client]
default-character-set=utf8
 
[mysqld]
port=3306
basedir=D:\\Program-Files\\mysql-8.0.32-winx64
# datadir=D:\\Program-Files\\mysql-8.0.32-winx64\\sqldata
max_connections=20
character-set-server=utf8
default-storage-engine=INNODB

[mysql]
default-character-set=utf8
```
### 配置环境变量，将其bin文件夹路径加入path里
### 在bin目录里运行 
```bash
# 运行之后会在根目录里生成data文件夹用来存放数据，以及会生成密码（需要自己先保存备用）
mysqld -I --console


```
## 安装 MySql 服务
```bash
mysqld install m8
```

## 启动服务
```bash
net start mysql8.0
```

## 测试连接
```bash
# 之后输入前面保存的密码
mysql -u root -p
```
## 重置密码
```bash
# 登录进去之后运行
set password = '123456';
```
此时密码就是123456了
