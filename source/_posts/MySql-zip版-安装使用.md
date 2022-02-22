
# MySql zip 安装使用

## MySql 配置安装

1. 到官网下载mysql zip 压缩包
2. 解压到自定义目录
3. 在 mysql 根目录创建 `my.ini` 配置文件
```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:/DK-davion/install/mysql-5.7.36-winx64/mysql-5.7.36-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:/DK-davion/install/mysql-5.7.36-winx64/mysql-5.7.36-winx64/data 
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证, 
# 现在使用的是“caching_sha2_password” 但是当前有很多数据库工具和链接包都不支持“caching_sha2_password”，
# 为了方便还是改回了“mysql_native_password”认证插件。
default_authentication_plugin=mysql_native_password

[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8

[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```
4. 切换到mysql的bin文件所在的目录, 初始化数据库 `mysqld --initialize --console`, 最后的会显示默认密码, 删掉初始化的 datadir 目录，再执行一遍初始化命令，又会重新生成的。
```
C:\developer\mysql-5.7.36-winx64\bin>mysqld --initialize --console
2022-02-20T16:01:37.865205Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is
 deprecated. Please use --explicit_defaults_for_timestamp server option (see doc
umentation for more details).
2022-02-20T16:01:38.657689Z 0 [Warning] InnoDB: New log files created, LSN=45790

2022-02-20T16:01:38.770889Z 0 [Warning] InnoDB: Creating foreign key constraint
system tables.
2022-02-20T16:01:38.853862Z 0 [Warning] No existing UUID has been found, so we a
ssume that this is the first time that this server has been started. Generating
a new UUID: 63182dab-9266-11ec-a772-5254002c56a2.
2022-02-20T16:01:38.859529Z 0 [Warning] Gtid table is not ready to be used. Tabl
e 'mysql.gtid_executed' cannot be opened.
2022-02-20T16:01:41.337226Z 0 [Warning] A deprecated TLS version TLSv1 is enable
d. Please use TLSv1.2 or higher.
2022-02-20T16:01:41.337889Z 0 [Warning] A deprecated TLS version TLSv1.1 is enab
led. Please use TLSv1.2 or higher.
2022-02-20T16:01:41.338919Z 0 [Warning] CA certificate ca.pem is self signed.
2022-02-20T16:01:41.644785Z 1 [Note] A temporary password is generated for root@
localhost: l+TGOykev0Nt   
```
5. 安装服务 `mysqld --install [服务名]`, 如果需要安装多个MySQL服务，就可以用不同的名字区分了，比如 mysql5 和 mysql8
6. 登录mysql `mysql -uroot -p`, 输入 `步骤4` 的默认密码
7. 修改默认密码 `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';`
8. 环境变量设置, 将 mysql 安装目录添加到 `Path` 中

## 执行 sql 文件

### Linux

1. `create database mydb`
2. `use database mydb`
3. `source /mydir/my.sql`

### Window

- 切换到mysql的bin文件所在的目录 `mysql -uroot -p123456 -Dtest<E:\test.sql` // mysql -u账号 -p密码 -D数据库名 < sql文件绝对路径
- 进入 MySQL 控制台, 执行  `source C:\test.sql`


## 开启MySQL远程访问权限 允许远程连接

1. use mysql;
2. select host, user, authentication_string from user;

原始数据

```
localhost	root			\*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9
localhost	mysql.session	\*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
localhost	mysql.sys		\*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
```

### 授权

授权后 `host` 为 `%`, 使用 `pass1234` 登录

1. use mysql;
2. GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'pass1234' WITH GRANT OPTION;
3. flush privileges;
4. select host, user, authentication_string from user;

```
localhost	root			\*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9
localhost	mysql.session	\*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
localhost	mysql.sys		\*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
%			root			\*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19
```

### 改表

1. use mysql;
2. update user set host = '%' where user = 'root';
3. select host, user, authentication_string from user;

```
localhost	root			\*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9
localhost	mysql.session	\*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
localhost	mysql.sys		\*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
%			root			\*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19
```

