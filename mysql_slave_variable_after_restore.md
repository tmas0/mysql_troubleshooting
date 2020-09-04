# MySQL slave configuration parameters

The following list should be reviewed when configuring a MySQL slave instance that you want restore a master backup.

## mysqld section

### innodb_flush_log_at_trx_commit

* Controls the balance between strict ACID compliance for commit operations and higher performance that is possible when commit-related I/O operations are rearranged and done in batches.
* With a setting of 0, logs are written and flushed to disk once per second. Transactions for which logs have not been flushed can be lost in a crash.
** In a restoration process, if the backup cannot be restored, the resynchronization fails, and another needs to be executed.

A valid value is:
```
innodb_flush_log_at_trx_commit=0
```

### innodb_buffer_pool_size

* The size in bytes of the buffer pool.
* The memory area where InnoDB caches table and index data.
* The default value is 128MB.
* For MyISAM tables, you can define the key_buffer_size.


A valid value is:
```
innodb_buffer_pool_size = RAM * 0.9
```

### innodb_log_file_size

* The size in bytes of each log file in a log group.
* The combined size of log files (innodb_log_file_size * innodb_log_files_in_group) cannot exceed a maximum value that is slightly less than 512GB.
* The default value is 48MB, and the default value of innodb_log_files_in_group is 2.

A valid value is:
```
innodb_log_file_size= RAM * 0.75
```

### innodb_io_capacity

* The innodb_io_capacity variable defines the number of I/O operations per second (IOPS) available to InnoDB background tasks.
* The default value is 200.
* For a higher-end, bus-attached SSD, consider a higher setting such as 1000, for example.
* For systems with individual 5400 RPM or 7200 RPM drives, you might lower the value to 100.

A valid value is:
```
innodb_io_capacity=700
```

### innodb_io_capacity_max

* If flushing activity falls behind, InnoDB can flush more aggressively, at a higher rate of I/O operations per second (IOPS) than defined by the innodb_io_capacity variable.
* The innodb_io_capacity_max variable defines a maximum number of IOPS performed by InnoDB background tasks in such situations.

A valid value is:
```
innodb_io_capacity_max=1500
```

### max_allowed_packet

* The maximum size of one packet or any generated/intermediate string, or any parameter sent by the mysql_stmt_send_long_data() C API function.
* The default is 64MB.
* The max value is 1GB.

A valid default value is:
```
max_allowed_packet=1GB
```

### innodb_doublewrite

* The innodb_doublewrite variable controls whether the doublwrite buffer is enabled.
* To disable the doublewrite buffer, set innodb_doublewrite to 0
* For master restoration, you should disable it to improve the performance.

A valid value is:
```
innodb_doublewrite=0
```

### performance_schema

* The value of this variable is ON or OFF to indicate whether the Performance Schema is enabled.
* By default, the value is ON.
* In a master restoration, you not need the performance stats.

A valid value:
```
performance_schema=0
```