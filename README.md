# database-migration-task

# 1. create MySQL in aws using rds source and destination.
![alt text](https://github.com/[username]/[reponame]/blob/[branch]/database using rds.jpg?raw=true)

# 2. create two ec2 instances source and destination and connect to rds database.
# 3. install aws cli in two ec2 servers and aws configure
# 4. install MySQL in two servers

# source ec2 server
---> connect and install mysql
---> connect to database with source endpoints from rds source database
     mysql -h database-1.clikeumg8ixe.us-east-1.rds.amazonaws.com -u admin -p

 # Create a Backup (Dump) of the Source Database
 ---> mysql dump -h source-db.c8wxyz1234.us-east-1.rds.amazonaws.com -u admin -p myappdb > myappdb_dump.sql
# Compress the file to save space
    gzip myappdb_dump.sql

# Transfer the Backup File to the Target Instance
	scp -i my-key.pem myappdb_dump.sql.gz ubuntu@54.158.185.204:/home/ubuntu/

# in target server verify
ls -l /home/ubuntu/

gunzip myappdb_dump.sql.gz

# Restore the Backup to the Target Database
mysql -h target-database.cluster-clikeumg8ixe.us-east-1.rds.amazonaws.com -u admin -p newappdb < myappdb_dump.sql

mysql -h target-database.cluster-clikeumg8ixe.us-east-1.rds.amazonaws.com -u admin -p


# using s3 buckets

# from source server Upload to S3
aws s3 cp myappdb_dump.sql s3://mysql-migration-bucket/

# Download on Target EC2
aws s3 cp s3://mysql-migration-bucket/myappdb_dump.sql .

# Restore to Target MySQL
mysql -h target-db.c9abcd5678.us-east-1.rds.amazonaws.com -u admin -p newappdb < myappdb_dump.sql

# Test and Verify
SHOW DATABASES;
USE newappdb;
SHOW TABLES;
SELECT * FROM users;


