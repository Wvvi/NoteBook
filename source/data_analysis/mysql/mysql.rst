.. contents::
   :depth: 3
..

简介
====

数据库操作语法笔记

数据库操作
==========

创建数据库

.. code:: mysql

    CREATE DATABASE dbname

查看数据库

.. code:: mysql

    show databases

选择数据库

.. code:: mysql

    USE dbname

查看数据库中的表

.. code:: mysql

    show tables

删除数据库

.. code:: mysql

    drop database dbname

常用数据类型
============

常用的数据类型，大致可以分为四类：数值型、浮点型、日期/时间和字符串（字符）类型

1. 数值类型

+----------------+---------+------------------+--------------------+
| 类型           | 字节    | 范围（signed）   | 范围（unsigned）   |
+================+=========+==================+====================+
| tinyint(n)     | 1字节   | -2^7 ~ 2^7-1     | 0 ~ 2^8-1          |
+----------------+---------+------------------+--------------------+
| smallint(n)    | 2字节   | -2^15 ~ 2^15-1   | 0 ~ 2^16-1         |
+----------------+---------+------------------+--------------------+
| mediumint(n)   | 3字节   | -2^23 ~ 2^23-1   | 0 ~ 2^24-1         |
+----------------+---------+------------------+--------------------+
| int(n)         | 4字节   | -2^31 ~ 2^31-1   | 0 ~ 2^32-1         |
+----------------+---------+------------------+--------------------+
| bigint(n)      | 8字节   | -2^63 ~ 2^63-1   | 0 ~ 2^64-1         |
+----------------+---------+------------------+--------------------+

-  显示宽度，如果某个数不够定义字段时设置的位数，则前面以0补填，zerofill
   属性修改 例：int(5) 插入一个数'123'，补填后为'00123'

2. 浮点数&定点数

+----------------+---------+------------------------------------+
| 类型           | 字节    | 说明                               |
+================+=========+====================================+
| float(M,D)     | 4字节   | 单精度浮点数（M总个数，D小数位）   |
+----------------+---------+------------------------------------+
| doubel(M,D)    | 8字节   | 双精度浮点数（M总个数，D小数位）   |
+----------------+---------+------------------------------------+
| decimal(M,D)   | M+2     | 定点数（M总个数，D小数位）         |
+----------------+---------+------------------------------------+

-  显示宽度，不同于整型，前后均会补0

-  浮点数表示近似值

-  对货币等对精度敏感的数据，应该用定点数表示或存储

-  编程中，如果用到浮点数，要特别注意误差问题，并尽量避免做浮点数比较

-  定点数保存一个精确的数值，不会发生数据的改变，不同于浮点数的四舍五入

-  将浮点数转换为字符串来保存，每9位数字保存为4个字节

-  decimal(m,d) 参数m<65 是总个数，d<30且 d<m 是小数位

3. 字符串类型&二进制类型

+--------------+-------------------------+--------------------+
| 类型         | 大小                    | 用途char           |
+==============+=========================+====================+
| char         | 0~255字节               | 定长字符串         |
+--------------+-------------------------+--------------------+
| varchar      | 0~65535字节             | 变长字符串         |
+--------------+-------------------------+--------------------+
| tinytext     | 0~255（2^8-1）字节      | 短文本字符串       |
+--------------+-------------------------+--------------------+
| text         | 0~65535（2^16-1）字节   | 长文本字符串       |
+--------------+-------------------------+--------------------+
| mediumtext   | 2^24-1字节              | 中等长度文本数据   |
+--------------+-------------------------+--------------------+
| longtext     | 2^32-1字节              | 极大文本数据       |
+--------------+-------------------------+--------------------+
| tinyblog     | 2^8-1字节               | 可存储二进制       |
+--------------+-------------------------+--------------------+
| bolg         | 2^16-1字节              | 可存储二进制       |
+--------------+-------------------------+--------------------+
| mediumblog   | 2^24-1字节              | 可存储二进制       |
+--------------+-------------------------+--------------------+

**char 和 varchar**\ ：

-  char(n)
   若存入字符数小于n，则以空格补于其后，查询之时再将空格去掉。所以 char
   类型存储的字符串末尾不能有空格，varchar 不限于此。
-  char(n) 固定长度，char(4) 不管是存入几个字符，都将占用 4
   个字节，varchar 是存入的实际字符数 +1
   个字节（n<=255）或2个字节(n>255)，所以 varchar(4),存入 3 个字符将占用
   4 个字节。
-  char 类型的字符串检索速度要比 varchar 类型的快

**varchar 和 text**\ ：

-  varchar 可指定 n，text 不能指定，内部存储 varchar 是存入的实际字符数
   +1 个字节（n<=255）或 2 个字节(n>255)，text 是实际字符数 +2 个字节
-  text 类型不能有默认值
-  varchar 可直接创建索引，text 创建索引要指定前多少个字符。varchar
   查询速度快于 text, 在都创建索引的情况下，text 的索引似乎不起作用

-  \_BLOB和\_text存储方式不同，\_TEXT以文本方式存储，英文存储区分大小写，而\_Blob是以二进制方式存储，不分大小写
-  \_BLOB存储的数据只能整体读出
-  \_TEXT可以指定字符集，\_BLO不用指定字符集

4. 时间日期类型

+-------------+---------+----------------------------------------------+
| 类型        | 字节    | 格式                                         |
+=============+=========+==============================================+
| datetime    | 8字节   | 1000-01-01 00:00:00 到 9999-12-31 23:59:59   |
+-------------+---------+----------------------------------------------+
| date        | 3字节   | 1000-01-01 到 9999-12-31                     |
+-------------+---------+----------------------------------------------+
| timestamp   | 4字节   | 19700101000000 到 2038-01-19 03:14:07        |
+-------------+---------+----------------------------------------------+
| time        | 3字节   | -838:59:59 到 838:59:59                      |
+-------------+---------+----------------------------------------------+
| year        | 1字节   | 1901 - 2155                                  |
+-------------+---------+----------------------------------------------+

数据表操作
==========

创建表

.. code:: mysql

    -- 样例
    CREATE TABLE IF NOT EXISTS `Scores`(
    `id` int(10) unsigned AUTO_INCREMENT,
    `student_no` varchar(10) not null default '' comment '学号',
    `grade` varchar(2) not null default '' comment '年级',
    `subject` varchar(10) not null default '' comment '科目',
    `score` tinyint(3) unsigned not null default '0' comment '分数',
    `ts` timestamp default current_timestamp on update current_timestamp,
    PRIMARY KEY (`id`)
        )ENGINE=InnoDB DEFAULT CHARSET=UTF8;

-  AUTO\_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1

-  ENGINE 设置存储引擎，CHARSET 设置编码

查看表

.. code:: mysql

    -- 查看所有表
    show tables [like 'pattern'];
    show tables from tablename;

    -- 查看表结构
    show create table tablename; \G
    DESC tablename;

**Alter** 操作

.. code:: mysql

    alter table tablename
    -- 添加列
    add column age int(3);

    -- 添加主键
    add primary key (`id`);
    add primary key (`id`,'age');

    -- 添加唯一索引，使得某字段的值不能重复(出null外，null可能会出现多次)
    add unique key `id` (`id`);

    -- 添加普通索引,索引值可出现多次
    add index index_name (`id`);

    -- 修改列类型
    modify grade varchar(10);

    -- 修改字段属性
    modify grade varchar(10);

    -- 更改字段名与类型
    change age new_age int(3);

    -- 更改表名
    rename [to] new_table;

    -- 删除字段
    drop age;

    -- 删除主键
    drop primary key;

    -- 删除索引
    drop index 索引名;

    -- 删除外键
    drop foreing key 外键;

-  主键具有唯一性
-  主键字段不能为null
-  主键可以由多个字段共同组成

其它

.. code:: mysql

    -- 删除表
    drop table tablename;

    -- 清空表数据
    truncate tablename;

    -- 复制表结构
    create table tablename select * from copy_tablename;

-  truncate 是删除表再创建，delete 是逐条删除
-  truncate 重置auto\_increment的值。而delete不会
-  当被用于带分区的表时，truncate 会保留分区

连接

.. code:: mysql

    -- inner join
    SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a INNER JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;

    -- left join
    SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a RIGHT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;

    -- right join
    SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a RIGHT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;

union

.. code:: mysql

    -- 用于将不同表中相同列中查询的数据展示出来（不包括重复数据）
    select country from websites union select country from apps order by country;

    -- 用于将不同表中相同列中查询的数据展示出来（包括重复数据）
    select country from websites union all select country from apps order by country;

内置函数
========

数值函数

.. code:: mysql

    abs(x)  -- 绝对值

    format(x,d)  -- 格式化千分位数 format(1234567.456, 2) = 1,234,567.46

    ceil(x)  -- 向上取整 ceil(10.1)=11

    floor(x)  -- 向下取整 floor(10.1)=10

    round(x)  -- 四舍五入

    mod(m,n)  -- m%n

    pi()  -- 获得圆周率

    pow(m,n)  -- m^n

    sqrt(x)  -- 算术平方根

    rand()  -- 随机数

    truncate(x,d)  -- 截取d位小数

时间日期函数

.. code:: mysql

    now(),current_timestamp()  -- 当前日期时间

    current_date()  -- 当前日期

    current_time()  -- 当前时间

    date('yyyy-mm-dd HH:MM:SS')  -- 获取日期部分

    time('yyyy-mm-dd HH:MM:SS')  -- 获取是时间部分

    date_format('yyyy-mm-dd HH:MM:SS', '%Y%m%d')  -- 格式化时间

    unix_timestamp()  -- 将时间转化为unix时间戳

    from_unixtime()  -- 将时间戳获得时间

-  目前timestamp 所能表示的范围在 1970 - 2038之间 ,超过这个范围
   得到的时间将会溢出 得到的时间是null

字符串函数

.. code:: mysql

    length(string)  -- string长度，字节

    char_length(string)  -- 字符个数

    substring(str,position [,length])  -- 从str的position开始,取length个字符

    replace(str,search_str,replace_str  -- 在str中用replace_str替换search_st

    instr(string ,substring)    -- 返回substring首次在string中出现的位置
            
    concat(string [,...])    -- 连接字串
            
    charset(str)            -- 返回字串字符集
            
    lcase(string)            -- 转换成小写
            
    left(string, length)    -- 从string2中的左边起取length个字符
            
    load_file(file_name)    -- 从文件读取内容
            
    locate(substring, string [,start_position])    -- 同instr,但可指定开始位置
            
    lpad(string, length, pad)    -- 重复用pad加在string开头,直到字串长度为length
            
    repeat(string, count)    -- 重复count次
            
    rpad(string, length, pad)    -- 在str后用pad补充,直到长度为length
            
    ltrim(string)            -- 去除前端空格
            
    rtrim(string)            -- 去除后端空格
            
    strcmp(string1 ,string2)    -- 逐字符比较两字串大小

流程函数

.. code:: mysql

    case when [condition] then result [when [condition] then result ...] [else result] end

    SELECT CASE                 -- 样例
    　　WHEN 1 > 0
    　　THEN '1 > 0'
    　　WHEN 2 > 0
    　　THEN '2 > 0'
    　　ELSE '3 > 0'
    　　END
