#aws-ec2-instance-snapshots 
 
## AWS ec2 script that makes snapshot for each attached volume and rotate it each day, week and month deleting older 
 
### This script makes;
* a snapshot of each volume attached to the selected amazon aws ec2 instance.
* check for snapshot older than KEEPFOR option ( -t ) seconds, 7 day default, and delete it 
* keep 1 snapshop for each day of the last week
* keep 1 snapshot each week  of the last month
* keep 1 snapshot each month
* All snapshot with description start equal to "AutoSnap:" will be rotated by this script
  
  
### setup

$ git clone --recursive https://github.com/fborraccetti/aws-ec2-instance-snapshots
$ cd aws-ec2-instance-snapshots/aws-sdk-for-php
$ cp config-sample.inc.php config.inc.php

edit config.inc.php and set access key and secret key from aws account security 

Look for your ec2 instance instance-id and go with php aws-ec2-instance-snapshots.php -i=YOURINSTANCEID -r=YOURREGION

  
### Parameters and options
* @param v

	The Instance ID of ec2 instance which you wish to manage.
* @param r

	( Optional ) Defaults to US-EAST-1.
	The region where the snapshots are held.
	Options include: us-e1, us-w1, us-w2, us-gov1, eu-w1, apac-se1, apac-ne1 AND sa-e1.
* @param t

	(Optional) Default to 604 800 seconds ( 7 days )
	The time in second to keep snapshot, snapshot older than will be deleted
* @param o

	(Optional) Defaults to TRUE.
	No operation mode, it won't *create* any snapshots. use -n -o to lock any create 				   and delete action
* @param q

	(Optional) Defaults to FALSE.
	Quiet mode, no ouput.
* @param n

	(Optional) Defaults to FALSE.
	No operation mode, it won't *delete* any snapshots. use -n -o to lock any create and delete action
 
### Example usage:
Do a snapshot of each volumes attached to instance with id i-7ed55c04 and delete snapshot older than 600 seconds

     ubuntu@test:~/aws-ec2-instance-snapshots$ php aws-ec2-instance-snapshots.php -i=i-7ed55c04 -r=us-e1 -t 600


###Crontab Example:

    00 00 * * * /usr/bin/php /home/fabio/admin/aws-ec2-instance-snapshots.php -i=i-7ed55c04
or
    00 00 * * * /usr/bin/php /home/ubuntu/aws-ec2-instance-snapshots/aws-ec2-instance-snapshots.php -i=i-66e22c2d -r=eu-w1

###Thanks
  some code snippet taken from:
  * https://github.com/michelem09/AWS-EC2-Manage-Snapshots-Backup
  * http://www.thecloudsaga.com/aws-ec2-manage-snapshots/
  Thank you for sharing your work!
  
WARNING : USE AT YOU OWN RISK!!! This application will delete snapshots with description starting with "AutoSnap:" unless you use the -o option
