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
  
  * Permissions : click on Json tab and paste the below code and Click Next

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

  * Policy Name : Enter a Policy Name accordingly and click next will Create a policy

      ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/cee137a5-cf1c-4d3f-836c-0eaf1c5e44ab)

# Create read/write glue service role
  
  * Go to IAM (Identity and Access Management) , click the policies tab on the left side of the page
  
       ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/3780f819-8178-4564-8922-63265ee1da0f)
    
  * click the Create policy

     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/d8efa108-87f2-4b2f-bb39-883eb53e857e)
    
  * Permissions : click on Json tab and paste the below code and Click Next

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
 
 * Policy Name: Enter a Policy Name accordingly and click next will Create a policy
 
      ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/cee137a5-cf1c-4d3f-836c-0eaf1c5e44ab)    

# Next, Create Crawler Roles for the policies created
	
  ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/b19ba65d-08e3-4310-b3b7-3d9cee24f545)

    * Next, select the trusted entity as below image, for trusted entity type, select AWS service and for the service or use case,select Glue and Click Next.

    * Add Permissions , select the policies created above and Click Next
    
     	
![image](https://github.com/jaykumsi/aws-glue/assets/137452836/c52b6e04-0079-4e6c-a9ca-12ed415067de)
    
    *  Role Name : Enter the role name accordingly and click Next will create a Role.

![image](https://github.com/jaykumsi/aws-glue/assets/137452836/62b34068-661e-4410-a4cf-f4bdcaa9c367)



## Create a Crawler to move the data from the S3 bucket to the Glue Data Catalog.

   * Go to AWS Glue, click the crawlers tab on the left side of the page ,click create crawler.

   ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/17221c31-3b79-4ef0-a6c0-d77f182787ae)

   * Set Crawler properties , Enter the unique crawler name  and click Next
     
![image](https://github.com/jaykumsi/aws-glue/assets/137452836/4e6906a7-b982-4bd1-ac78-a5f222a2a3cd)

   * Choose the Data sources,click on Add a data source
     
     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/1b5e4962-ad08-4890-a2be-7fa5b62082f2)

   * Choose the data source to be S3,Network Connection -Optional, and give the S3 bucket path in S3 path and Click Next

     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/58b00314-9d8e-4035-9ccd-b78d2a9a870a)

   * Configure Security Setting, We already created the IAM role, use that and click Next.

     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/3cc6ba17-8ae8-4869-a587-12e038c03195)

   *  Set Output and Scheduling, for Target Database, select the database which is created or click Add Database
     
     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/cd541952-1e5c-4793-a211-e6b5d1dd812a)

   * Crawler Schedule, you can select the crawler schedule on how to run it, below you can choose anyone.
     
       ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/cf25233d-a92d-4fa6-91d1-82d5626ff9cf)

   * Once you select scheduler, then click next and review the full setting before clicking finish. It will create a crawler to move the data from S3 to Data 	 
     Catalog

   * Once the Crawler is created, please select Run Crawler and once it is completed it will display as below.

     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/78a416e1-8b61-48cc-a845-12f97e167986)

     ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/3fa2ddbc-6a7e-4462-9e3f-eb10ec8d8515)




  ## Classifier

   * A classifier reads the data in a data store. If it recognizes the format of the data, it generates a schema. The classifier also returns a certainty number 
     to indicate how certain the format recognition was.
   * AWS Glue provides a set of built-in classifiers, but you can also create custom classifiers. AWS Glue invokes custom classifiers first, in the order that you 
     specify in your crawler definition. Depending on the results that are returned from custom classifiers, AWS Glue might also invoke built-in classifiers. If a 
     classifier returns certainty=1.0 during processing, it indicates that it's 100 percent certain that it can create the correct schema. AWS Glue then uses the 
     output of that classifier.
  
   * If no classifier returns certainty=1.0, AWS Glue uses the output of the classifier that has the highest certainty. If no classifier returns a certainty 	 
     greater than 0.0, AWS Glue returns the default classification string of UNKNOWN.

   * Classifiers are triggered during a crawl task. A classifier checks whether a given file is in a format it can handle. If it is, the classifier creates a 
     schema in the form of a StructType object that matches that data format.

   * You can use the standard classifiers that AWS Glue provides, or you can write your own classifiers to best categorize your data sources and specify the 
     appropriate schemas to use for them. A classifier can be a grok classifier, an XML classifier, a JSON classifier, or a custom CSV classifier, as specified in 
     one of the fields in the Classifier object.

  # Contents

   * CsvClassifier
	A classifier for comma-separated values (CSV).
	Type: CsvClassifier object
   * GrokClassifier
	A classifier that uses grok.
	Type: GrokClassifier object
   * JsonClassifier
	A classifier for JSON content.
	Type: JsonClassifier object
   * XMLClassifier
	A classifier for XML content.
	Type: XMLClassifier object
  
  # When to use Classifier
  *  You use classifiers when you crawl a data store to define metadata tables in the AWS Glue Data Catalog. You can set up your crawler with an ordered set of 
     classifiers. When the crawler invokes a classifier, the classifier determines whether the data is recognized. If the classifier can't recognize the data or 
     is not 100 percent certain, the crawler invokes the next classifier in the list to determine whether it can recognize the data.

  # Types of Classifier :
   * Custom classifiers
   * Built-In classifers

   * Custom classifier : 
     			The output of a classifier includes a string that indicates the file's classification or format (for example, json) and the schema of the 			file. For custom classifiers, you define the logic for creating the schema based on the type of classifier. Classifier types include 				defining schemas based on grok patterns, XML tags, and JSON paths.
			If you change a classifier definition, any data that was previously crawled using the classifier is not reclassified. A crawler keeps 				track of previously crawled data. New data is classified with the updated classifier, which might result in an updated schema. If the 				schema of your data has evolved, update the classifier to account for any schema changes when your crawler runs. To reclassify data to 				correct an incorrect classifier, create a new crawler with the updated classifier.
   * Built-In classfier :
                        AWS Glue provides built-in classifiers for various formats, including JSON, CSV, web logs, and many database systems.

			If AWS Glue doesn't find a custom classifier that fits the input data format with 100 percent certainty, it invokes the built-in 				classifiers in the order shown in the following table. The built-in classifiers return a result to indicate whether the format matches 				(certainty=1.0) or does not match (certainty=0.0). The first classifier that has certainty=1.0 provides the classification string and 				schema for a metadata table in your Data Catalog.

			Files in the following compressed formats can be classified:

			  ZIP (supported for archives containing only a single file). Note that Zip is not well-supported in other services (because of the 					archive).
			  BZIP
			  GZIP
			  LZ4
			  Snappy (supported for both standard and Hadoop native Snappy formats)

   * Built-in CSV classifier
			The built-in CSV classifier parses CSV file contents to determine the schema for an AWS Glue table. This classifier checks for the 				following delimiters:

			  Comma (,)
			  Pipe (|)
			  Tab (\t)
			  Semicolon (;)
			  Ctrl-A (\u0001)
			  Ctrl-A is the Unicode control character for Start Of Heading.

		       To be classified as CSV, the table schema must have at least two columns and two rows of data. The CSV classifier uses a number of 			       heuristics to determine whether a header is present in a given file. If the classifier can't determine a header from the first row of data, 		       column headers are displayed as col1, col2, col3, and so on. The built-in CSV classifier determines whether to infer a header by evaluating 
                       the following characteristics of the file:

		       Every column in a potential header parses as a STRING data type.
		       Except for the last column, every column in a potential header has content that is fewer than 150 characters. To allow for a trailing 			       delimiter, the last column can be empty throughout the file.

		       Every column in a potential header must meet the AWS Glue regex requirements for a column name.
		       The header row must be sufficiently different from the data rows. To determine this, one or more of the rows must parse as other than 			       STRING type. If all columns are of type STRING, then the first row of data is not sufficiently different from subsequent rows to be used as 		       the header.

     * Below are the steps to create a classifier
          
	    ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/4ff73b46-7650-48b5-b4f6-d92a3acbfbf8)

     

	
