# binlog日志补充
>对于binlog来说mysql-bin.xxx就是binlog的节点日志文件，而mysql-bin.index这是日志文件的所以会记录的是所有日志文件的地址

在data目录下执行命令:
````
[root@localhost data]# more mysql-bin.index     
./mysql-bin.000001
./mysql-bin.000002
./mysql-bin.000003
./mysql-bin.000004
./mysql-bin.000005
./mysql-bin.000006
./mysql-bin.000007
./mysql-bin.000008
./mysql-bin.000009
./mysql-bin.000010   ...........
````
或者命令行中
````
mysql> show binary logs;
+------------------+-----------+-----------+
| Log_name         | File_size | Encrypted |
+------------------+-----------+-----------+
| mysql-bin.000001 |      7731 | No        |
| mysql-bin.000002 |      2250 | No        |
| mysql-bin.000003 |       512 | No        |
| mysql-bin.000004 |       178 | No        |
| mysql-bin.000005 |       178 | No        |
| mysql-bin.000006 |       178 | No        |
| mysql-bin.000007 |      2658 | No        |
| mysql-bin.000008 |       178 | No        |
| mysql-bin.000009 |       178 | No        |
| mysql-bin.000010 |       178 | No        |
| mysql-bin.000011 |       178 | No        |
| mysql-bin.000012 |       178 | No        |
| mysql-bin.000013 |       178 | No        |
| mysql-bin.000014 |       178 | No        |
| mysql-bin.000015 |  12414194 | No        |
| mysql-bin.000016 |       155 | No        |
| mysql-bin.000017 |       178 | No        |
| mysql-bin.000018 |      1676 | No        |
| mysql-bin.000019 |      1545 | No        |
+------------------+-----------+-----------+
19 rows in set (0.02 sec)
````
## 我们可以看看binlog日志中的信息

### 方式一:
````
mysql> show binlog events in 'mysql-bin.000019'\G;  
*************************** 1. row ***************************
   Log_name: mysql-bin.000019
        Pos: 4
 Event_type: Format_desc
  Server_id: 1
End_log_pos: 124
       Info: Server ver: 8.0.19, Binlog ver: 4
*************************** 2. row ***************************
   Log_name: mysql-bin.000019
        Pos: 124
 Event_type: Previous_gtids
  Server_id: 1
End_log_pos: 155
       Info:              .............
````
### 方式二:
````
[root@localhost ~]# mysqlbinlog /usr/local/mysql/data/mysql-bin.000019 
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
# at 4
#200520 23:44:50 server id 1  end_log_pos 124 CRC32 0xeb2fdd56  Start: binlog v 4, server v 8.0.19 created 200520 23:44:50 at startup
# Warning: this binlog is either in use or was not closed properly.
ROLLBACK/*!*/;
BINLOG '
MvnFXg8BAAAAeAAAAHwAAAABAAQAOC4wLjE5AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAy+cVeEwANAAgAAAAABAAEAAAAYAAEGggAAAAICAgCAAAACgoKKioAEjQA
CgFW3S/r
'/*!*/;
# at 124             ...........
````
### 查看binlog的信息,如果是 宝塔安装 先设置mysqlbinlog为系统[环境变量](https://www.cnblogs.com/kevingrace/p/8072860.html)
````
[root@localhost data]# vi ~/.bash_profile
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
# User specific environment and startup programs
PATH=$PATH:$HOME/bin:/www/server/mysql/bin
export PATH

[root@localhost data]# source ~/.bash_profile
[root@localhost data]# cd /www/server/data
[root@localhost data]# mysqlbinlog ./mysql-bin.000009
````

``mysql> show binlog events in 'mysql-bin.000001';``
