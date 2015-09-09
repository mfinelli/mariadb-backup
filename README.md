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

```ini
[mysqldump]
user=backupuser
password=backupuserpassword
```

## Usage

Running a backup is easy! After configuring everything just run the
`dbbackup` script!

Note that after decrypting and decompressing a backup you can restore it like
so (this will prompt you for the user's password):

```shell
$ mysql -u user -p database < database.sql
```

### Crontab
You might also find it helpful to run the backup as a cron job. Below is an
example that runs the backup everyday at 0245, but you could obviously adjust
it to suit your needs. I also have my s3 bucket set to delete backups older
than three months. Assuming you have the script deployed to your home
directory:

```
45 2 * * * cd /home/user/maria-backup && /home/user/maria-backup/dbbackup
```
