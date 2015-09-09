# MariaDB Backup

Backup MariaDB/MySQL databases to S3.

## Dependencies

### s3cmd

[s3cmd](https://github.com/s3tools/s3cmd) is used to actually upload the
backups to AWS, so you will need to install and configure it first. Install
it with your favorite package manager and then configure with:

```shell
$ s3cmd --configure
```

## Configuration

Configuration for the database backup  is saved in your home directory in the
`.dbbackup` file. It is basically just another bash script that defines the
variables that we need which we source before running the backup.

You need to define three variables: a password with which to encrypt the
backups before uploading to S3, a list of databases to backup and the name of
an S3 bucket where the backups will be saved.

```shell
# Choose a long, strong password to encrypt the backups.
PASSPHRASE="SomeReallyGoodPassword"

# List of databases to backup:
DBS=('test_database')

# You can list your buckets with `s3cmd ls`.
BUCKET="your-backup-bucket"
```

### `mysqldump`

You also need to make sure that the username and password for the `mysqldump`
user is configured in one of the standard locations: `/etc/my.cnf`,
`/etc/mysql/my.cnf`, or `~/.my.cnf`. Currently, the script does no checking
to make sure the values are set but if they aren't then `mysqldump` will
obviously fail.

```
[mysqldump]
user=backupuser
password=backupuserpassword
```
