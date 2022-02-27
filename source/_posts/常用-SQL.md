---
title: 常用 Sql 总结
date: 2022-01-25 10:44:38
categories:
	- SQL
tags:
	- MySql
description: "常用 Sql 总结"
copyright: true
---


# MySql

-- 表所有列, 用`,`分割
SELECT GROUP_CONCAT(COLUMN_NAME SEPARATOR ",") FROM information_schema.COLUMNS WHERE TABLE_SCHEMA = 'ry' AND TABLE_NAME = 'fws_activity_user';


-- 查看表结构
SELECT TABLE_SCHEMA,TABLE_NAME,COLUMN_NAME,column_type,column_comment, 'as', COLUMN_NAME 
FROM information_schema.COLUMNS 
WHERE TABLE_SCHEMA = 'ry' AND TABLE_NAME like 'fws_s%' order by TABLE_NAME;


-- 查看已存在的表
SELECT TABLE_SCHEMA, table_name,table_comment, create_time FROM information_schema.`TABLES` WHERE TABLE_SCHEMA = 'ry' order by table_name;


-- 查看已存在表的 建表语句
show create table fws_service_fee;


-- 日期类
select NOW(),CURRENT_TIMESTAMP(),SYSDATE(),date_format('2022-02-12 14:56','%Y-%m-%d'), unix_timestamp('2022-02-12'), from_unixtime(1644595200, '%Y-%m-%d %H:%i');

select NOW(),CURRENT_TIMESTAMP(),SYSDATE(),date_format('2022-02-12 14:56','%Y-%m-%d'), unix_timestamp(SYSDATE()), from_unixtime(1644595200, '%Y-%m-%d %H:%i');

select unix_timestamp(SYSDATE()), from_unixtime(1644595200, '%Y-%m-%d %H:%i'),unix_timestamp(SYSDATE()) + 3600, unix_timestamp(SYSDATE()) + 3000;


-- 格式化金额
select convert(123.456789123, DECIMAL(12,4));


-- 保存 emoji 表情
-- 修改表
ALTER TABLE fws_service_fee CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
-- 修改列
alter table fws_service_fee modify column store_name varchar(50) character set utf8mb4 collate utf8mb4_unicode_ci default null;
ALTER TABLE fws_service_fee CHANGE store_name store_name varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
ALTER TABLE fws_service_fee CHANGE user_name user_name varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
