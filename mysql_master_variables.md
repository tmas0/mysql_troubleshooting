# MySQL configuration parameters

The following list should be reviewed when configuring a MySQL instance.

## mysqld section

### port

* The number of the port on which the server listens for TCP/IP connections.
* Default value is 3306

How to set it correctly?
* If your database is exposed to the internet jungle, please change the standard port.
** If you have this case, please rethink and redefine your architecture. It seems incorrect and insafe.
* If your instance it's in a DMZ, please, you don't need change the standard port.

When you don't known, a valid default value is:
```
port = 3306
```

### tmp_table_size

* The maximum size of internal in-memory temporary tables.
* Default value is 16MB.

How to set it correctly?
* Check your temporary data.
* Check your number of tables.
* You can compare the number of internal on-disk temporary tables created to the total number of internal temporary tables created by comparing Created_tmp_disk_tables and Created_tmp_tables values.

You can see more information about [internal temporary tables](https://dev.mysql.com/doc/refman/8.0/en/internal-temporary-tables.html)

When you don't known, a valid default value is:
```
tmp_table_size = 16MB
```

### sort_buffer_size

* Space used on ORDER BY and GROUP BY operations.
* Each session that must perform a sort allocates a buffer of this size.

How to set it correctly?
* At minimum the sort_buffer_size value must be large enough to accommodate fifteen tuples in the sort buffer.
* There are thresholds of 256KB and 2MB where larger values may significantly slow down memory allocation, so you should consider staying below one of those values.
* Max value of this variable is 4GB.

When you don't known, a valid default value is:
```
sort_buffer_size = 1MB
```

### join_buffer_size

* The minimum size of the buffer that is used for plain index scans, range index scans, and joins that do not use indexes and thus perform full table scans.
* The default is 256KB.

How to set it correctly?
* The best way to get fast joins is to add indexes. Increase the value of join_buffer_size to get a faster full join when adding indexes is not possible.


When you don't known, a valid default value is:
```
join_buffer_size = 256KB
```

### innodb_buffer_pool_size

* The size in bytes of the buffer pool.
* The memory area where InnoDB caches table and index data.
* The default value is 128MB.
* For MyISAM tables, you can define the key_buffer_size.

How to set it correctly?
* Check your available RAM.
* Define if your server is a dedicated server or not.
** If you have a dedicated instance, you must enable innodb_dedicated_server, and the value of innodb_buffer_pool_size is automatically configured.
* Buffer pool size must always be equal to or a multiple of innodb_buffer_pool_chunk_size * innodb_buffer_pool_instances.

When you don't known, a valid default value is:
```
If you have a >= v.8
innodb_dedicated_server = ON
elif RAM <= 1GB
innodb_buffer_pool_size = 128MB
elif RAM > 1GB and RAM <= 4GB
innodb_buffer_pool_size = RAM * 0.5
else
innodb_buffer_pool_size = RAM * 0.75 if all your tables are InnoDB.
```

### default_storage_engine

* The default storage engine for tables.
* This variable sets the storage engine for permanent tables only.

How to set it correctly?
* The default value is InnoDB.
* If you consider install [MyRocks](http://myrocks.io/), please, set it as a default storage engine.

A valid value is:
```
default_storage_engine = InnoDB
```

### innodb_flush_method

* Defines the method used to flush data to InnoDB data files and log files, which can affect I/O throughput.
* On Unix-like systems, the default value is fsync.
* On Windows, the default value is unbuffered.
* When innodb_dedicated_server is enable, this variable is automatically configured.

When you don't known, a valid default value is:
```
On Unix-like:
innodb_flush_method = 4
```

### character_set_client

* The character set for statements that arrive from the client.
* The session value of this variable is set using the character set requested by the client when the client connects to the server.
* The global value of the variable is used to set the session value in cases when the client-requested value is unknown or not available, or the server is configured to ignore client requests.

When you don't known, a valid default value is:
```
character_set_client = utf8mb4
```

### max_connections

* The maximum permitted number of simultaneous client connections.
* The default value is 151.
* mysqld actually permits max_connections + 1 client connections. The extra connection is reserved for use by accounts that have the CONNECTION_ADMIN privilege.
* Increasing the max_connections value increases the number of file descriptors that mysqld requires.

How to set it correctly?
* The quality of the thread library on a given platform.
* The amount of RAM available.
* The amount of RAM is used for each connection.
* The workload from each connection.
* The desired response time.
* The number of file descriptors available.

When you don't known, a valid default value is:
```
max_connections = 150
```

### max_allowed_packet

* The maximum size of one packet or any generated/intermediate string, or any parameter sent by the mysql_stmt_send_long_data() C API function.
* The default is 64MB.

When you don't known, a valid default value is:
```
max_allowed_packet = 16MB
```

### binlog_expire_logs_seconds

* Sets the binary log expiration period in seconds.
* After their expiration period ends, binary log files can be automatically removed.
* Possible removals happen at startup and when the binary log is flushed.
* The default binary log expiration period is 2592000 seconds, which equals 30 days (30*24*60*60 seconds).
* Introduced in version 8, the old variable was named expire_logs_days.

When you don't known, a valid default value is:
```
binlog_expire_logs_seconds = 3600 # 1 day
```

### innodb_file_per_table

* When innodb_file_per_table is enabled, tables are created in file-per-table tablespaces by default.
* When disabled, tables are created in the system tablespace by default.

When you don't known, a valid default value is:
```
innodb_file_per_table = ON
```

### key_buffer_size

* Buffer of keys only for MyISAM tables.

When you don't known, a valid default value is:
```
key_buffer_size = 16MB
```

### read_buffer_size

* Each thread that does a sequential scan for a MyISAM table allocates a buffer of this size (in bytes) for each table it scans.
* If you do many sequential scans, you might want to increase this value.
* The value of this variable should be a multiple of 4KB.

When you don't known, a valid default value is:
```
read_buffer_size = 1MB
```