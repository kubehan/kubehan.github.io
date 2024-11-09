---
title: MySQL相关操作MySQL相关操作
author: Kubehan
type: post
date: 2020-02-21T09:18:06+08:00
url: /287.html
wb_dl_type:
  - 0
  - 0
views:
  - 696
  - 696
Baidusubmit:
  - 1
zm_like:
  - 1
categories:
  - Linux运维

---
<!--python安装手册开始-->

<!--python安装手册结束-->

<!--####专栏广告位图文切换开始-->

<!--####专栏广告位图文切换结束-->

<div id="article_content" class="article_content clearfix">
  <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/ck_htmledit_views-833878f763.css" />
  
  <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/ck_htmledit_views-833878f763.css" />
  
  <div class="htmledit_views" id="content_views">
    <h1 class="postTitle" style="width:690px;border:none;color:rgb(0,0,0);font-family:'微软雅黑';background-color:rgb(199,203,189);">
      MySQL相关操作
    </h1>
    
    <div class="clear" style="background-color:rgb(199,203,189);clear:both;color:rgb(0,0,0);font-family:'微软雅黑';font-size:12px;">
    </div>
    
    <div class="postBody" style="background-color:rgb(199,203,189);width:690px;border:none;color:rgb(73,73,73);font-family:Arial, Helvetica, sans-serif;font-size:14px;line-height:1.6;">
      <div id="cnblogs_post_body" class="blogpost-body" style="background-color:transparent;">
        <p>
            一、库操作（相当于文件夹）
        </p>
        
        <p>
            1、创建库
        </p>
        
        <p>
             create database 库名 engine=存储引擎 charset 字符编码;  #常用存储引擎为innodb,其他还有myisam(存取速度快但支持的功能少),memory(数据保存在内存),blackhole(数据直接丢弃)；存储引擎和字符编码可以不写，使用配置文件参数
        </p>
        
        <p>
             2、删除库
        </p>
        
        <p>
             drop database 库名
        </p>
        
        <p>
             3、修改库的设置
        </p>
        
        <p>
             alter database 库名 engine=存储引擎 charset 字符编码;
        </p>
        
        <p>
             4、查看MySQL中已经创建了哪些库
        </p>
        
        <p>
             show databases;
        </p>
        
        <p>
             5、查看当前在那个库中
        </p>
        
        <p>
             select database();
        </p>
        
        <p>
             6、查看创建指定库时的sql语句
        </p>
        
        <p>
             show create database 库名;
        </p>
        
        <p>
             7、切换到指定库
        </p>
        
        <p>
             use 库名;
        </p>
        
        <p>
             8、mysql默认库的作用
        </p>
        
        <p>
             information_schema： 虚拟库，不占用磁盘空间，存储的是数据库启动后的一些参数，如用户表信息、列信息、权限信息、字符信息等<br />   performance_schema： MySQL 5.5开始新增一个数据库：主要用于收集数据库服务器性能参数，记录处理查询请求时发生的各种事件、锁等现象 <br />   mysql： 授权库，主要存储系统用户的权限信息，其中user表用户具有所有库的指定权限，db表用户有某些库的指定权限，tables_priv表用户有指定库下的某些表的指定权限，columns_prive表用户有指定库下的指定表的某些字段的指定权限<br />   test： MySQL数据库系统自动创建的测试数据库
        </p>
        
        <p>
           
        </p>
        
        <p>
             二、表操作（相当于文件）
        </p>
        
        <p>
             1、创建表
        </p>
        
        <p>
              create table 表名(字段名1 数据类型(宽度) 约束条件,字段名2 数据类型(宽度) 约束条件,.....);  #约束条件中primary key、foreign key、unique key可以在所有字段之后定义，多用于定义组合键，宽度和约束条件不是必须条件
        </p>
        
        <p>
              2、删除表
        </p>
        
        <p>
              drop table 表名;
        </p>
        
        <p>
              3、修改表的设置
        </p>
        
        <p>
                3.1、添加字段
        </p>
        
        <p>
                alter table 表名 add 字段名 数据类型(宽度) 约束条件 (after 字段名2/first); #after表示在指定字段后面创建新字段，first表示将新字段放在第一位
        </p>
        
        <p>
                3.2、修改字段定义/重定义字段
        </p>
        
        <p>
                alter table 表名 modify 字段名 新数据类型(新宽度) 新约束条件;#用于修改字段定义或者不做修改只是重新定义字段使表的存储引擎和字符编码的修改生效
        </p>
        
        <p>
                3.3、修改表的参数
        </p>
        
        <p>
                alter table 表名 engine=新存储引擎  charset 新字符编码;#如果表中已有数据需要重新定义字段才能使修改生效
        </p>
        
        <p>
                3.4、修改字段名
        </p>
        
        <p>
                alter table 表名 change 旧字段名 新字段名 新数据类型(新宽度) 新约束条件;#可以修改字段名或者重新定义字段
        </p>
        
        <p>
                3.5、删除字段
        </p>
        
        <p>
                alter table 表名 drop 字段名;
        </p>
        
        <p>
                3.6、修改表名
        </p>
        
        <p>
                alter table 旧表名 rename  新表名;
        </p>
        
        <p>
                3.7、增加复合主键
        </p>
        
        <p>
                alter table 表名 add primary key();
        </p>
        
        <p>
             4、查看当前库中存在哪些表
        </p>
        
        <p>
             show tables;
        </p>
        
        <p>
             5、查看创建表时的sql语句
        </p>
        
        <p>
             show create table 表名;
        </p>
        
        <p>
             6、查看指定表的表结构
        </p>
        
        <p>
             desc 表名;
        </p>
        
        <p>
             7、复制表
        </p>
        
        <p>
             create table 表名 select 字段名1,字段名2,... from 表名2 where 条件;#复制表2的表结构以及查询到的数据
        </p>
        
        <p>
             create table 表名 select 字段名1,字段名2,... from 表名2 where 不成立的条件;#只复制表2的表结构
        </p>
        
        <p>
           
        </p>
        
        <p>
            三、MySQL数据类型
        </p>
        
        <p>
            1、数字
        </p>
        
        <p>
             整型（宽度表示显示的数据长度）
        </p>
        
        <p>
             tinyint   表示范围：有符号-128~127  无符号：0~255
        </p>
        
        <p>
             int   表示范围：有符号-2147483648~2147483647   无符号0~4294967295
        </p>
        
        <p>
             bigint  表示范围：int类型无法表示的数字
        </p>
        
        <p>
           
        </p>
        
        <p>
             浮点型(数据总长度,保留小数位数)
        </p>
        
        <p>
             float   精确度最小，足够日常使用
        </p>
        
        <p>
             double  精确度中等
        </p>
        
        <p>
             decimal  精确度最高
        </p>
        
        <p>
           
        </p>
        
        <p>
            2、字符
        </p>
        
        <p>
            char  存放与宽度相等的字符数，实际数据字符数小于宽度时用空格在后面补足，存取速度快，浪费空间
        </p>
        
        <p>
            varchar  按实际数据的字符数存放，存取速度慢，节省空间
        </p>
        
        <p>
           
        </p>
        
        <p>
            3、日期时间
        </p>
        
        <p>
            datetime  完整的日期时间
        </p>
        
        <p>
            date   只存日期
        </p>
        
        <p>
            time   只存时间
        </p>
        
        <p>
            year  只存年份
        </p>
        
        <p>
            now()函数可以换取当前的日期时间并按定义的时间类型截取相应部分
        </p>
        
        <p>
            在插入时间日期数据时只需要数据按年月日时分秒的顺序写入，格式不影响写入数据
        </p>
        
        <p>
           
        </p>
        
        <p>
            4、枚举
        </p>
        
        <p>
            enum('','',...)  只能选其中一个，不选择则默认给第一个值，传入数据不在范围内则该列不显示数据
        </p>
        
        <p>
            set('','',...) 可以选择一个或多个，传入数据不在范围内则该列不显示数据
        </p>
        
        <p>
           
        </p>
        
        <p>
            四、约束条件
        </p>
        
        <p>
            primary key  设置主键
        </p>
        
        <p>
            foreign key  设置外键
        </p>
        
        <p>
            unique key  设置该列值必须唯一
        </p>
        
        <p>
            not null  设置该列不能为空
        </p>
        
        <p>
            default  设置默认值   default '默认值'
        </p>
        
        <p>
            auto_increment  自增
        </p>
        
        <p>
            unsigned  数字类型数字设置为无符号数
        </p>
        
        <p>
            zerofill  数字类型长度小于显示长度时用0在左边填充
        </p>
        
        <p>
           
        </p>
        
        <p>
            五、数据操作（相当于文件内容）
        </p>
        
        <p>
            1、插入数据
        </p>
        
        <p>
             insert into 表名 values(),(),...;#每条数据必须包含所有字段的值，并按字段定义顺序传递
        </p>
        
        <p>
             insert into 表名(字段名1,字段名2,...) values(),(),...;#每条数据只需要传递指定字段的值，其余字段使用默认值，没有默认值则报错
        </p>
        
        <p>
            2、删除数据
        </p>
        
        <p>
            delete from 表名 where 条件; #删除某些数据
        </p>
        
        <p>
            truncate 表名;清空表
        </p>
        
        <p>
            3、修改数据
        </p>
        
        <p>
            update 表名 set 字段名1=字段值1,字段值2=字段值2,... where 条件;
        </p>
        
        <p>
            4、单表查询数据
        </p>
        
        <p>
            select (distinct) 字段名1,字段名2,... from 表名 (where 条件/group by 字段名/having 条件/order by 字段名/limit 数字);
        </p>
        
        <p>
            支持对查询字段进行+,-,*,/,%操作然后显示其结果，并且可以给该结果另起一个字段名来显示，如SELECT name, salary*12 AS Annual_salary FROM employee;
        </p>
        
        <p>
            5、多表查询数据
        </p>
        
        <p>
              5.1、内连接
        </p>
        
        <p>
              select 字段名1,字段名2,...  from  表1 inner join 表2 on 表1.字段=表2.字段 
        </p>
        
        <p>
              5.2、左连接（可以不断使用left join连接n多张表）
        </p>
        
        <p>
              select 字段名1,字段名2,...  from 表1 left join 表2 on 表1.字段=表2.字段  
        </p>
        
        <p>
              5.3、右连接
        </p>
        
        <p>
              select 字段名1,字段名2,... from 表1 right join 表2 on 表1.字段=表2.字段  
        </p>
        
        <p>
              5.4、全连接（利用union关键字）
        </p>
        
        <p>
              select 字段名1,字段名2,... from 表1 left join 表2 on 表1.字段=表2.字段 union select 字段名1,字段名2,... from 表1 right join 表2 on 表1.字段=表2.字段 
        </p>
        
        <p>
            6、子查询
        </p>
        
        <p>
            子查询就是将一个sql语句的结果作为另一个sql语句的条件值
        </p>
        
        <p>
             select * from employee where dep_id in (select id from department where name in ('技术','销售')); 
        </p>
        
        <p>
           
        </p>
        
        <p>
            六、关键字执行优先级及使用
        </p>
        
        <p>
            from > on > join > where > group by > 按照select后的字段取值，有聚合就做聚合>having > select > distinct > order by > limit
        </p>
        
        <p>
            group by：显示每个分组中的内容使用group_concat()函数，如SELECT post,GROUP_CONCAT(name) FROM employee GROUP BY post;
        </p>
        
        <p>
            distinct：去掉重复的数据，如SELECT DISTINCT post FROM employee; 
        </p>
        
        <p>
            order by：可以按照多行排序,如order by name desc,age asc，排序标准按照指定的顺序，前一个字段的值相等则按照后面一个字段的值排序，以此类推；asc结果按升序排列，desc结果按降序排列
        </p>
        
        <p>
            limit：规定显示数据的数量，可以指定两个值，第一个值表示显示的起始数据位置，第二个值表示一共显示的数据数量，如limit 5,5表示显示从第六条数据开始往后的五条数据
        </p>
        
        <p>
           
        </p>
        
        <p>
            七、MySQL中的条件表达式
        </p>
        
        <p>
             数值比较：>,<,=,>=,<=,!=，between 数字1 and 数字2  #结果包含边界值
        </p>
        
        <p>
             逻辑比较：or,and,not,in,not in,is null,is not null
        </p>
        
        <p>
             模糊查询：like ''  其中可以使用通配符，_表示匹配单个任意字符，%表示匹配任意多个的任意字符
        </p>
        
        <p>
             正则表达式：regexp '正则表达式'
        </p>
        
        <p>
           
        </p>
        
        <p>
             八、聚合函数
        </p>
        
        <p>
             count(),max(),min(),avg(),sum() ,group_concat()#用于对指定字段执行相关操作，如SELECT SUM(salary) FROM employee WHERE depart_id=3;
        </p>
        
        <p>
           
        </p>
        
        <p>
            九、索引
        </p>
        
        <p>
            索引分类：1、唯一索引 primary key  unique  2、普通索引  index  3、联合索引 将多个字段组合成为一个索引
        </p>
        
        <p>
            创建索引：1、建表时创建，唯一索引可以在字段后添加或者在所有字段后添加；普通索引只能在所有字段后添加index(字段名1,字段名2,......)  2、建表后创建  create  index  索引名  on 表名 (字段名1,字段名2,.....)  
        </p>
        
        <p>
            删除索引：drop index  索引名  on 表名(字段名) #加字段名删除某个字段的索引，不加字段删除整个索引
        </p>
        
        <p>
            适合建立索引的字段：1、字段的数据量比较小  2、字段中的数据区分度高
        </p>
        
        <p>
            最左匹配：索引匹配按照索引字段定义顺序从左往右匹配，匹配条件中必须有定义索引时的第一个字段，因此在创建索引时应将有明确条件值的字段放在左边其余放右边
        </p>
        
        <p>
            使用索引的方式：1、在查询语句中将索引字段作为约束条件并且给出明确的条件值或者一个较小的范围  2、匹配条件中的索引字段不能参与计算或使用函数对其操作  3、匹配条件中的索引字段对应的数据类型与数据库中存放的该字段的数据类型不一致就无法使用索引，除非该字段为主键字段或者其数据类型为整数类型  3、order by中排序依据为索引字段时，select也需要有该字段，否则无法使用索引，除非该字段为主键
        </p>
        
        <p>
            覆盖索引：查询字段和条件字段都是相同的索引字段，此时查询效率最高
        </p>
        
        <p>
            索引合并：多个单独的索引字段同时作为匹配条件字段
        </p>
        
        <p>
            and：当多个条件字段用and连接时，如果有些字段的区分度比较低，会从左往右一直找可以区分的字段
        </p>
        
        <p>
           
        </p>
        
        <p>
            十、高效sql语句注意事项
        </p>
        
        <p align="left">
              <span style="font-size:15px;">避免使用select *</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">    count(1)或count(列) 代替 count(*)</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">    创建表时尽量时 char 代替 varchar</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">    表的字段顺序固定长度的字段优先</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">    组合索引代替多个单列索引（经常使用多个条件查询时）</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">    尽量使用短索引</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">   使用连接（JOIN）来代替子查询(Sub-Queries)</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">   连表时注意条件类型需一致</span>
        </p>
        
        <p align="left">
          <span style="font-size:15px;">   索引散列值（重复少）不适合建索引，例：性别不适合</span>
        </p>
        
        <pre></pre>

<p>
     
</p>

<p>
     十 一、慢查询优化步骤
</p>

<pre><span lang="en-us" style="font-size:15px;" xml:lang="en-us">0.先运行看看是否真的很慢，注意设置<span lang="en-us" xml:lang="en-us">SQL_NO_CACHE</span></span></pre>

<pre><span lang="en-us" style="font-size:15px;" xml:lang="en-us">1.where条件单表查，锁定最小返回记录表。这句话的意思是把查询语句的<span lang="en-us" xml:lang="en-us">where都应用到表中返回的记录数最小的表开始查起，单表每个字段分别查询，看哪个字段的区分度最高</span></span></pre>

<pre><span lang="en-us" style="font-size:15px;" xml:lang="en-us">2.explain查看执行计划，是否与<span lang="en-us" xml:lang="en-us">1预期一致（从锁定记录较少的表开始查询）</span></span></pre>

<pre><span lang="en-us" style="font-size:15px;" xml:lang="en-us">3.order by limit 形式的<span lang="en-us" xml:lang="en-us">sql语句让排序的表优先查</span></span></pre>

<pre><span lang="en-us" style="font-size:15px;" xml:lang="en-us">4.了解业务方使用场景</span></pre>

<pre><span lang="en-us" style="font-size:15px;" xml:lang="en-us">5.加索引时参照建索引的几大原则</span></pre>

<pre><span lang="en-us" style="font-size:14px;" xml:lang="en-us"><span style="font-size:15px;">6.观察结果，不符合预期继续从</span><span lang="en-us" xml:lang="en-us"><span style="font-size:15px;">0分析</span>
十二、慢查询日志配置
</span></span></pre>

<pre><span style="font-size:14px;"><span style="font-size:15px;">show variables like '%query%';
show variables like '%queries%';
set global 变量名 = 值</span>
十三、数据备份恢复
1、单库备份
mysqldump -uroot -p123 库名  表名1 表名2 ... &gt; XXX.sql
2、多库备份
mysqldump -uroot -p123 --databases 库名1 库名2 ... &gt;XXX.sql
3、全库备份
mysqldump -uroot -p123 --all-databases &gt; XXX.sql
4、数据库恢复
mysql -uroot -p123 库名 &lt; XXX.sql
十四、数据导入导出
</span><span style="font-size:15px;">1、导入数据</span>
<span style="font-size:15px;">load data infile '文件路径' into table 表名 fields terminated by '分隔符' lines terminated by '分隔符';</span>
<span style="font-size:15px;">2、导出数据</span>
<span style="font-size:15px;">select * from 表名 into outfile '文件路径' fields terminated by '分隔符' lines terminated by '分隔符';</span>
<span style="font-size:15px;">5.6及以上版本需要修改配置文件/etc/my.cnf,在[mysqld]下加上secure-file-priv=数据导出路径</span>
<span style="font-size:15px;">十五、命令行执行sql语句</span>
<span style="font-size:15px;">mysql -uroot -p123 -e 'use db1；show tables;'</span></pre>

<p>
   
</p>

<p>
     十六、视图
</p>

<p>
      创建视图
</p>

<p>
      create  view  视图名   as  select语句  #将单表或多表查询结果以一张虚拟表的形式保存下来，只有表结构没有自己的数据
</p>

<p>
      删除视图
</p>

<p>
      drop  view  视图名
</p>

<p>
      修改视图表结构
</p>

<p>
      alter  view  视图名  as   新的select语句
</p>

<p>
   
</p>

<p>
     十七、触发器
</p>

<p>
      创建触发器
</p>

<p>
      create  trigger  触发器名   before/after   insert/update/delete   on  表名  for  each  row    begin     。。。。      end;   #涉及触发器关联表数据的操作，可以用new或old提取其中指定的字段值，new表示即将插入的数据，old表示即将删除的数据
</p>

<p>
     
</p>

<p>
       删除触发器
</p>

<p>
       drop  trigger  触发器名
</p>

<p>
   
</p>

<p>
     十八、事务
</p>

<p>
      事务只对数据相关的sql操作有效
</p>

<p>
      start  transaction;
</p>

<p>
      sql语句
</p>

<p>
      rollback;  #sql语句出现异常执行rollback回滚，取消开启事务后的所有sql操作
</p>

<p>
      commit; #使之前的sql语句生效
</p>

<p>
   
</p>

<p>
   
</p>

<p>
      十九、存储过程
</p>

<p>
      创建存储过程
</p>

<pre><span style="font-size:15px;">delimiter //
create procedure p4(
    inout n1 int
)
BEGIN
    select * from blog where id &gt; n1;
    set n1 = 1;
END //
delimiter ;</span></pre>

<p>
   
</p>

<p>
   
</p>

<p>
      查看存储过程
</p>

<p>
      show create procedure auto_insert1G
</p>

<p>
   
</p>

<p>
      使用存储过程
</p>

<pre><span style="font-size:15px;">set @x=3;  #设置全局变量
call p4(@x);  #存储过程正常执行后将会修改全局变量x
select @x;  #查询当前全局变量的值，看存储过程是否正常执行完毕
二十、函数
需要掌握的内置函数
date_format('2017-07-20 22:22:22','%Y-%m') #输出结果为2017-07
自定义函数
</span></pre>

<pre><span style="font-size:15px;">delimiter //
create function f1(
    i1 int,
    i2 int)
returns int
BEGIN
    declare num int;
    set num = i1 + i2;
    return(num);
END //
delimiter ;</span></pre>

<pre><span style="font-size:15px;">执行函数
</span></pre>

<pre><span style="font-size:15px;">select f1(11,nid) ,name from tb2;</span></pre>

<pre><span style="font-size:15px;">
删除函数
drop  function  函数名
二十一、流程控制
if判断语句
if 条件项 条件  条件值 then
　　sql语句1
elseif </span><span style="font-size:14px;">条件项 条件  条件值 then
　　sql语句2
else
    sql语句3
end if;
while循环语句
while </span><span style="font-size:14px;">条件项 条件  条件值 do
　　sql语句
end while;
</span></pre>

<p>
   
</p>

<p>
     二十二、其他
</p>

<p>
     concat()函数定义显示格式，如select concat('<名字:',name,'>  ','<  薪资:',salary,'>') as employee_info from employee;#as表示将查询结果以指定的列名显示
</p>

<p>
     CONCAT_WS()  作用类似concat，第一个参数为分隔符。如SELECT CONCAT_WS(':',name,salary*12) AS Annual_salary FROM employee;
</p>

<p>
     
</p>

<p>
     创建数据库用户
</p>

<p>
     create user '用户名'@'允许登陆的IP' identified by '登陆密码'
</p>

<p>
     
</p>

<p>
     查看指定操作的帮助信息
</p>

<p>
     help  操作  如 help grant
</p>

<p>
     
</p>

<p>
    授权
</p>

<p>
    grant 操作权限(字段名1,字段名2,....)  on  库名.表名 to '用户名'@'允许登陆的IP'  identified by '密码'; #操作权限有select,update,delete等，字段名可加可不加，库名和表名可以是具体的也可以用*指代所有的，允许登陆的IP中可以使用%指代任意IP或者某个网段如192.168.20.%，加上identified by可以创建用户。
</p>

<p>
    取消授权
</p>

<p>
    revoke 操作权限(字段名1,字段名2,....)  on  库名.表名 from 用户名@'允许登陆的IP';
</p>

<p>
   
</p>

<p>
    一对多、一对一、多对多定义
</p>

<p>
    一对多：关联表中设置外键的列不唯一
</p>

<p>
    一对一：关联表中设置外键的列唯一
</p>

<p>
    多对多：当两个表的每条数据都对应另一张表中的多条数据时表示这两张表之间存在多对多关系，需要新建一张关系表用于存储这两张表之间的关联关系，一般关系表中设置外键的列需要设置联合唯一
</p>

<p>
   
</p>

<p>
    if函数：有三个参数，第一个参数是判断条件，第二个参数是条件成立时的值，第三个参数是条件不成立时的值，如if(isnull(ty),0,ty)表示当体育成绩为空时值为0，不为空时值为体育的成绩
</p>