## Glue Crawler
  * A Program that connects to your data source (S3 ,DyDB) to scan your data and creates metadata tables in Glue Data Catalog
  * Can scan multuple data sources in single run
  * Once completed,it will create table in Data catalog
  * This table can be used in Athena,ETL jobs,Redshift Spectrum

![image](https://github.com/jaykumsi/aws-glue/assets/137452836/133157d9-b3fc-4716-b7b0-6d7c3ed06863)
  
## Data Sources that Glue Crawlers can crawl
  * Native client
    * S3:
    * Dynamo DB:
  * JDBC 
      * Amazon Redshift
      * Snowflake
      * Amazon Aurora
      * MariaDB
      * Microsoft SQL Server
      * MySQL
      * Oracle
      * PostgreSQL
  * MangoDB client 
      * Mongo DB
      * MongoDB Atlas
      * Amazon DocumentDB

    
# Create read/write policy for S3 bucket 
  * Go to IAM (Identity and Access Management) , click the policies tab on the left side of the page
       ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/3780f819-8178-4564-8922-63265ee1da0f)
  * click the Create policy
     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/d8efa108-87f2-4b2f-bb39-883eb53e857e)
  * Permissions : click on Json tab and paste the below code and click Next
        {
    "Version": "2012-10-17",
    "Statement": [
        {
              "Effect": "Allow",
              "Action": [
                  "s3:GetObject",
                  "s3:PutObject"
              ],
              "Resource": "arn:aws:s3:::tini-d-gluebucket-001*"
        }
               ]
        }
     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/6960191b-34db-4b9c-ac47-f9a2afcf8aeb)

  * Policy Name : Enter a Policy Name accordingly and click next will create a policy
      ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/cee137a5-cf1c-4d3f-836c-0eaf1c5e44ab)

# Create read/write glue service role
  * Go to IAM (Identity and Access Management) , click the policies tab on the left side of the page
       ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/3780f819-8178-4564-8922-63265ee1da0f)
  * click the Create policy
     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/d8efa108-87f2-4b2f-bb39-883eb53e857e)
  * Permissions : click on Json tab and paste the below code and click Next

{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"glue:*",
				"s3:GetBucketLocation",
				"s3:ListBucket",
				"s3:ListAllMyBuckets",
				"s3:GetBucketAcl",
				"iam:ListRolePolicies",
				"iam:GetRole",
				"iam:GetRolePolicy",
				"cloudwatch:PutMetricData"
			],
			"Resource": [
				"*"
			]
		},
		{
			"Effect": "Allow",
			"Action": [
				"s3:GetObject",
				"s3:PutObject"
			],
			"Resource": [
				"arn:aws:s3:::aws-glue-*/*",
				"arn:aws:s3:::*/*aws-glue-*/*"
			]
		},
		{
			"Effect": "Allow",
			"Action": [
				"s3:GetObject"
			],
			"Resource": [
				"arn:aws:s3:::crawler-public*",
				"arn:aws:s3:::aws-glue-*"
			]
		},
		{
			"Effect": "Allow",
			"Action": [
				"logs:CreateLogGroup",
				"logs:CreateLogStream",
				"logs:PutLogEvents"
			],
			"Resource": [
				"arn:aws:logs:*:*:*:/aws-glue/*"
			]
		}
	]
}
     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/88d6e300-5b28-4c94-9dab-280905d1fc21)
 * Policy Name : Enter a Policy Name accordingly and click next will ##create a policy
      ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/cee137a5-cf1c-4d3f-836c-0eaf1c5e44ab)    
