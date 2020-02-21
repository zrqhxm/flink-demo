---
layout:     post
title:      maxcompute
subtitle:   ETL
date:       2019-05-06
author:     willis
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
   - maxcompute
   - 笔记
---

### writer插件

```shell
mongo Writer插件 写数据前删除指定数据
"preSql":{
    "type":"remove",
    "item":[
        {
            "name":"day",
            "value":"${delday}", # delday=${yyyy-mm-dd}
            "condition":"$gt" # 不配置时默认是$et等于
        }
    ]
},
```

### 深圳区maxcompute集群IP

```
100.106.46.0/24,100.106.49.0/24,10.152.27.0/24,10.152.28.0/24,11.192.91.0/24,11.192.96.0/24,11.193.103.0/24,100.64.0.0/10,120.76.104.0/24,120.76.91.0/24,120.78.45.0/24 
资源组ip
192.168.206.214
192.168.206.9
```

### 表结构操作

```sql
#修改列名
ALTER TABLE table_name CHANGE COLUMN old_col_name RENAME TO new_col_name;
#添加列
ALTER TABLE table_name ADD COLUMNS (col_name1 type1 comment 'XXX',col_name2 type2 comment 'XXX');
#修改列注释
ALTER TABLE table_name CHANGE COLUMN col_name COMMENT comment_string;
#修改列名及注释
ALTER TABLE table_name CHANGE COLUMN old_col_name new_col_name column_type COMMENT column_comment;
#动态分区
INSERT OVERWRITE TABLE PARTITION (pt)  #pt和表的分区字段相同 注意pt的顺序
select a.*,pt from table 
```

### sql函数







### 参数配置

```
#获取的是业务时间
ystoday=${yyyymmdd} |  bizdate=$bizdate
ysday=${yyyy-mm-dd}
bizmonth=$bizmonth  #获取业务月  上一个月
ysmonth=${yyyymm}	#获取昨天的月
#常量值
key1="00:00:00"
key2="23:59:59"

eg:
created_time between '${ysday} ${key1}' and '${ysday} ${key2}'  # 参数拼接
created_time < '2019-11-01 00:00:00'
created_time between '2019-11-01 00:00:00' and '2019-11-01 23:59:59'
pt=20191101
```

### ODPS SOL参数配置

```
#允许分区表全表扫描
set odps.sql.allow.fullscan=true;
```

### 权限

```
use cen_jl_text; --打开项目空间。
add user aliyun$dataworks@aliyun.com; --添加用户。
add user ram$dataworks@aliyun.com:Allen; --添加RAM子账号。
create role worker; --创建角色。
grant worker TO aliyun$alice@aliyun.com; --角色指派。
grant worker TO ram$bob@aliyun.com:Allen;  --角色指派。
grant CreateInstance, CreateResource, CreateFunction, CreateTable, List ON PROJECT test_project_a TO ROLE worker; --对角色授权。

account_name:dataworks
```

### tunnel操作

```
#下载资源到本地  添加 -c 'gbk' 可以更改csv文件格式不乱码
tunnel download instance://20200214025542798gkzus592 d:\data\store_in_out.txt -c 'gbk';
tunnel download test_project.test_table/p1="b1",p2="b2" log.txt   //下载指定表数据
tunnel download instance://test_project/test_instance log.txt     //下载指定Instance的执行结果
```

### 自定义资源组服务器

```sehll
#更新资源组服务器  可以在更新前先将StartUp进程kill掉

#step1：
wget https://di-agent-image-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/install.sh --no-check-certificate

#step2:
sh install.sh --user_name=zz_1ae1f26562b2412487913865e923a6c2 --password=1cio7ls8to25bbatotdmd23d --enable_uuid=true  --region_id=cn-shenzhen --bucket=di-agent-image-cn-shenzhen  --time_zone=GMT+8 --deploy_locale=zh_CN

#step3: 查看是否成功更新并启动服务
tail -f /home/admin/alisatasknode/logs/alisatasknode.log
tail -f /home/admin/alisatasknode/logs/heartbeat.log
```

