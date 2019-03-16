## Welcome to GitHub Pages

This program will backup file paths and MySQL databases to Amazon's S3 storage cloud service.

I've used this program in production for over a year with no problems. But, your results may vary.

## Features
- Simple to use
- Backups all databases on a MySQL server
- Backups and compresses (using bzip2) directories and files
- Uses URL type storage keys, so it's easy to browse the backup bucket
- (optionally) Removes old backups according to a grandfather-father-son based schedule

## Installation
- Copy the backup.dist.php file to a new file name, like backup.php
- Add your Amazon AWS access key, password, and bucket in the backup.php file provided.
- Customize the files and database servers that are backed up.
- You may optionally list any number of .php files underneath the "backups" folder. Each one will be executed.
- Upload to your server.
- Setup a cronjob to run the backups for you!

For example, create a file called /etc/cron.daily/backup and add this code to it:
```
#!/bin/bash
/usr/bin/php /path/to/script/backup.php    
exit 0
```
NOTE: Make sure to set the /etc/cron.daily/backup file to be executable. Like this:
```
chmod +x /etc/cron.daily/backup
```
## About this script
This set of scripts ships with three files:

- backup.dist.php--This is an example of how to call the backup functions
- include/
- -backup.inc.php--All the backup functions are in here
- -S3.php--This is the library to access Amazon S3
 
## (optional) Daily backup deletion schedule
A unique feature of this script is the way it will retain (and eventually delete) old backups to conserve space, and yet maintain significant backup history. In this method you will have the following full backups:

- Everyday for the past two weeks (14 backups)
- Every saturday for the past 2 months (8 or 9 backups)
- First day of the month going back forever
- This allows you to keep a very detailed history of your files during the most recent time, but progressively remove backups as time goes on to save space. This feature can be turned off if you want to store all your backups, all the time.

To disable this feature, comment out the following line from backup.inc.php:
```
deleteBackups($BACKUP_BUCKET);
```
## Hourly backups
You can optionally run the backups every hour. Hourly backups will be available for the last 72 hours. Beyond that, Midnight (00) hour backups will be available according to the daily backup deletion schedule outlined above.

##Weekly backup deletion schedule
You can optionally run the backups each week. Weekly backups will be retained according to the following schedule:

- Each week going back 12 weeks (~3 months) (12 backups)
- Each month going back 36 weeks (~9 months) (9 backups)
- One backup per year going back forever

## Configuration
There are several constants that may be set to change the function of the backup script. Below is a list of them.

## Required
- awsAccessKey - string - Your Amazon Access Key. Required.
- awsSecretKey - string - Your Amazon Secret Key. Required.
- awsBucket - string - The name of the bucket to put backups into. Required.
- schedule - string - One of "hourly", "daily", or "weekly". Required.
 
## Optional
- awsEndpoint - string - Set the endpoint URL of the server that speaks the S3 API. Default is s3.amazonaws.com. Optional.
- debug - boolean - Whether or not to emit debug messages. Default is false. Optional.
- mysqlDumpOptions - string - Any options that you want to send to the mysqldump command during backups. Optional.
- schedule - string - One of: 'weekly', 'daily', or 'hourly' - Tells the script how often you are backing up so that it can do the right thing to remove old backups.
- timezone - string - System timezone, default is 'America/Los_Angeles', Optional.
 
## Requirements
- PHP 5 or higher
- PHP curl
- PHP mysql
- GNU/Linux environment
- Other POSIX environments should work, but not tested
 
## About the author
My name is Ujwal Abhishek. I wrote this small program to help backup my virtual machines hosted at various places. I figured someone might be able to make use of it, so I've published it as open source. I also wanted a way to store my version history, learn to use git, and do it for free. Thanks github!

## License
This program is licensed under the MIT license. See the include/backup.inc.php file for a copy of the license.

## ToDo
Add ability to use S3 Reduced Redundancy Storage to save money.