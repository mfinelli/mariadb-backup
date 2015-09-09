# MariaDB Backup

Backup MariaDB/MySQL databases to S3.

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
