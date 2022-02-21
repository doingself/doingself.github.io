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
