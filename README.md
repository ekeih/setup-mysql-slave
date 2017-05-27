# Setup MySQL replication

This playbook will setup a MySQL replication by doing the following steps:

1. Backup the current databases from the MySQL master
2. Compress the backup
3. Download the backup
4. Upload and uncompress the backup to the MySQL slave
5. Stop MySQL on the slave
6. Delete all old MySQL databases from the MySQL slave
7. Import the backup
8. Start MySQL on the slave
9. Configute the MySQL slave process
10. Start the MySQL slave process

The slave will use SSL to connect to the master. So you have to make sure that your master has a valid SSL certificate that can be validated by a CA from `/etc/ssl/certs` on the slave. (Checkout [Letsencrypt](https://letsencrypt.org/))

WARNING: This playbook will delete all previous data from the MySQL slave!

## Requirements

- A working MySQL installation on the master with valid SSL setup.
- A replication User on the master. (`GRANT REPLICATION SLAVE ON *.* TO 'SOMEIP'@'SLAVEHOST';`) You may also want to use `REQUIRE SSL`.
- [innobackupex](https://www.percona.com/doc/percona-xtrabackup/2.1/innobackupex/innobackupex_script.html) to backup the master.
- A working MySQL installation on the slave.

## Options

| Option                     | Default | Description |
|----------------------------|---------|-------------|
| master                     |         | The MySQL master from your Ansible inventory. |
| slave                      |         | The MySQL slave from your Ansible inventory. All MySQL data from this host will be deleted. |
| mysql_replication_master   | master  | The Hostname or IP that will be set as MASTER_HOST in MySQL. |
| mysql_replication_user     |         | MASTER_USER in MySQL. |
| mysql_replication_password |         | MASTER_PASSWORD in MySQL. |

## Example

Assuming that you defined `mysql_replication_user` and `mysql_replication_password` in your host_vars you can simply run:

```
ansible-playbook main.yml -e 'master=master.exmaple.com slave=slave.example.com'
```
