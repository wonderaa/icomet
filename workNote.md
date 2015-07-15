**Linux网卡配置**

- ONBOOT=yes
- BOOTPROTO=static
- IPADDR=192.168.0.175
- NETMASK=255.255.255.0
- GATEWAY=192.168.0.1
- DNS1=114.114.114.114
- MM_Controlled=no
- DEVICE=eth0

**svn 启动命令**

`serversvn -d -r responsePath`

**tar打包过滤`.svn`文件**

`tar -zcvf p2p.tar.gz p2p --exclude "*.svn"`

**MySql主从配置**

1、主服务器开启log-bin日志

2、在主服务器上设置读锁有效，确保没有数据库操作，以便获得一个一致性的快照

`mysql->flush tables with read lock;`

3、mysqldump 备份数据

`mysqldump -uroot -p dbname -l -F>/tmp/test.sql`

*-l锁定 -F 重新生成bin-log日志*

拷贝到从服务器

`scp /tmp/test.sql root@'ip':/tmp/`

4、解锁

`unlock tables;`

**MySql从服务器配置**

1、设置从服务器server-id`不能与主服务器相同`

2、导入数据库

	mysqldump -uroot -p dbname < /tmp/test.sql

3、修改从服务器配置文件

	master-host=''

	master-user=''

	master-password=''

4、重启MySql

	pkill mysqld

	service mysqld start

5、启动从服务器复制进程

	slave start

6、查看是否配置成功

	show slave status
	1、Slave_IO_Running: Yes
	2、Slave_SQL_Running: Yes
	3、Master_Log_File: mysql-bin.000003
	4、Exec_Master_Log_Pos

   `查看上述信息是否正确`	

7、如果有错误可通过如下命令修改:

	1、slave stop;
	2、change master to 
		master_host='',
		master_user='',
		master_password='',
		master_log_file="",
		master_log_pos=;
	3、slave start
**`或者`**

	1、slave stop;
	2、SET GLOBAL SQL_SLAVE_SKIP_COUNTER=1;
	3、slave start;