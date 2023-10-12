=>master steps<=
vim /etc/mysql/mysql.conf.d/mysqld.cnf
[mysqld]
# bind-address = 127.0.0.1 (find this and comment this out)
server_id           = 1
log_bin             = /var/log/mysql/mysql-bin.log
binlog_format=ROW
log_bin_index       = /var/log/mysql/mysql-bin.log.index
max_binlog_size     = 100M
expire_logs_days    = 1
systemctl restart mysql -> in system
systemctl status mysql
=> in mysql database
mysql> create user 'replica_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Junior@dev';
mysql> grant replication slave on *.* to 'replica_user'@'%';
mysql> flush privileges;
mysql> show master status\G;
*************************** 1. row ***************************
             File: mysql-bin.000002
         Position: 157
     Binlog_Do_DB:
 Binlog_Ignore_DB:
Executed_Gtid_Set:
1 row in set (0.00 sec)
=>slave steps<=
vim /etc/mysql/mysql.conf.d/mysqld.cnf
[mysqld]
# bind-address = 127.0.0.1 (find this and comment this out)
server_id           = 2
systemctl restart mysql -> in system
mysql> change master to master_host='10.0.0.211', master_port=3306, master_user='replica_user',master_password='Junior@dev', master_log_file='mysql-bin.000002', master_log_pos=157;
mysql> start slave;
mysql> show slave status\G;
check in status:
        Relay_Master_Log_File: mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
