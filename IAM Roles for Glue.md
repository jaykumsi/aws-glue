When working with AWS Glue, you will need Identity and Access Management (IAM) roles to grant permissions for various operations within the service. Here are some typical IAM roles and the permissions they might need:

## AWS Glue Console Role:

This role is used by individuals who access the AWS Glue Console.
Permissions might include AWSGlueConsoleFullAccess to allow users to manage AWS Glue resources through the AWS Management Console.
```sql
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetBucketLocation",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": [
                "arn:aws:s3:::your-source-bucket",
                "arn:aws:s3:::your-target-bucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::your-source-bucket/*",
                "arn:aws:s3:::your-target-bucket/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "glue.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:ListMetrics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "logs:CreateLogGroup",
            "Resource": "arn:aws:logs:region:account-id:log-group:/aws/glue/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:region:account-id:log-group:/aws/glue/*:log-stream:*"
        }
    ]
}
```

## S3 IAM Role

IAM roles in the context of Amazon S3 (Simple Storage Service) are used to define and manage permissions for entities such as EC2 instances, Lambda functions, or other AWS services to interact with S3 buckets securely. Below, I'll provide an overview of how you can create IAM roles specifically for S3:

Example 1: Granting EC2 Instances Access to an S3 Bucket
1. Create an IAM Role:
      * Use the IAM console, AWS CLI, or SDKs to create an IAM role.
      * Define the trusted entity as EC2.

2. Attach S3 Policy:
      * Attach an S3 policy to the IAM role to define the permissions. For read-only access, you can use a policy like:

![image](https://github.com/jaykumsi/aws-glue/assets/137452836/07dcde37-5b77-4b5c-abdd-d8afb1f7ac39)

{
  "Version": "2012-10-17",
  "Statement": [
{
 "Effect": "Allow",
 "Action": "s3:GetObject",
 "Resource": "arn:aws:s3:::your-bucket-name/*"
}
]
}

      * Replace "your-bucket-name" with the actual name of your S3 bucket.

3. Launch EC2 Instance with IAM Role:
     * When launching an EC2 instance, specify the IAM role you created.

Example 2: Granting Lambda Function Access to an S3 Bucket

1. Create an IAM Role:
   * Use the IAM console, AWS CLI, or SDKs to create an IAM role.
   * Define the trusted entity as Lambda.

2. Attach S3 Policy:

   * Attach an S3 policy to the IAM role. For read and write access, you can use a policy like:

![image](https://github.com/jaykumsi/aws-glue/assets/137452836/5cec80c3-319b-41dd-82f7-7814ec74bd96)

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
     "s3:GetObject",
       "s3:PutObject"
   ],
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}

   * Replace "your-bucket-name" with the actual name of your S3 bucket.

3. Specify IAM Role in Lambda Configuration:
    * When creating or updating your Lambda function, specify the IAM role you created.

Example 3: Cross-Account Access to an S3 Bucket
1. Create IAM Role (Account A - Source Account):
     * Define a role with trust relationship allowing another account to assume it:
 
  ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/ea89e634-046f-48f6-8985-70f3869e913b)

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::DESTINATION-ACCOUNT-ID:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {}
    }
  ]
}
 * Replace "DESTINATION-ACCOUNT-ID" with the AWS account ID of the destination account.

2. Create IAM Role (Account B - Destination Account):
    * Create an IAM role with the necessary S3 permissions.

3. Assume Role in Lambda or EC2 (Account A):
    * Use the STS AssumeRole API to assume the role created in Account B.
    
These examples provide a basic framework for setting up IAM roles with S3. Always customize the policies based on your specific use case and adhere to the principle of least privilege to ensure secure access to your S3 resources.







## AWS Glue ETL Job Role:

This role is used by AWS Glue ETL jobs to read from and write to AWS resources during the ETL process.
Permissions might include:
Read access to the source data (e.g., Amazon S3 buckets, databases, or other data stores).
Write access to the target data (e.g., Amazon S3, Amazon Redshift).
Permissions to access necessary AWS Glue resources.
Execution permissions for AWS Glue ETL jobs.
```sql
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:StartJobRun",
                "glue:GetJobRun",
                "glue:GetJobRuns",
                "glue:GetJobs",
                "glue:BatchStopJobRun",
                "glue:UpdateJob",
                "glue:GetJob",
                "glue:CreateJob",
                "glue:DeleteJob",
                "glue:BatchDeleteJob",
                "glue:GetUserDefinedFunction",
                "glue:GetUserDefinedFunctions"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::your-source-bucket",
                "arn:aws:s3:::your-source-bucket/*",
                "arn:aws:s3:::your-target-bucket",
                "arn:aws:s3:::your-target-bucket/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:region:account-id:log-group:/aws/glue/*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudwatch:PutMetricData",
            "Resource": "*"
        }
    ]
}
```


## AWS Glue Crawling Role:

This role is used by AWS Glue crawlers to discover and catalog metadata about your source data.
Permissions might include:
Read access to the source data.
Write access to the AWS Glue Data Catalog.
Permissions to access necessary AWS Glue resources.

```sql
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:StartCrawler",
                "glue:GetCrawler",
                "glue:GetCrawlers",
                "glue:CreateCrawler",
                "glue:UpdateCrawler",
                "glue:DeleteCrawler",
                "glue:GetCrawlerMetrics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::your-source-bucket",
                "arn:aws:s3:::your-source-bucket/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetDatabase",
                "glue:CreateDatabase"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetTable",
                "glue:GetTables",
                "glue:CreateTable"
            ],
            "Resource": "arn:aws:glue:region:account-id:catalog/defaultdatabase/defaulttable"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:region:account-id:log-group:/aws/glue/*"
        }
    ]
}
```
## AWS Glue DevEndpoint Role:

This role is used by AWS Glue DevEndpoints, which are development environments for running and testing ETL scripts.
Permissions might include:
Read and write access to source and target data.
Access to AWS Glue resources.
Permissions to create and manage temporary resources.
```sql
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateDevEndpoint",
                "glue:DeleteDevEndpoint",
                "glue:GetDevEndpoint",
                "glue:GetDevEndpoints",
                "glue:UpdateDevEndpoint",
                "glue:ListDevEndpoints",
                "glue:StartDevEndpoint",
                "glue:StopDevEndpoint"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::your-source-bucket",
                "arn:aws:s3:::your-source-bucket/*",
                "arn:aws:s3:::your-target-bucket",
                "arn:aws:s3:::your-target-bucket/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateNetworkInterface",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DeleteNetworkInterface"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:region:account-id:log-group:/aws/glue/*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudwatch:PutMetricData",
            "Resource": "*"
        }
    ]
}
```
## AWS Glue Service Role:

This role is used by the AWS Glue service itself to perform various tasks, such as managing ETL jobs, accessing the Data Catalog, and creating temporary resources.
Permissions might include:
Access to AWS Glue resources.
Permissions to read from and write to the Data Catalog.
Permissions to create and manage temporary resources.
These are general guidelines, and the specific permissions needed depend on your use case and the AWS Glue features you are using. AWS provides managed policies like AWSGlueServiceRole and AWSGlueConsoleFullAccess that you can attach to your roles to grant the necessary permissions.
```sql
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateDatabase",
                "glue:DeleteDatabase",
                "glue:GetDatabase",
                "glue:GetDatabases",
                "glue:UpdateDatabase",
                "glue:BatchDeleteTable",
                "glue:BatchDeleteTableVersion",
                "glue:CreateTable",
                "glue:DeleteTable",
                "glue:GetTable",
                "glue:GetTables",
                "glue:UpdateTable",
                "glue:CreatePartition",
                "glue:DeletePartition",
                "glue:BatchDeletePartition",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:UpdatePartition",
                "glue:BatchCreatePartition",
                "glue:CreateTableVersion",
                "glue:GetTableVersion",
                "glue:GetTableVersions",
                "glue:UpdateTableVersion",
                "glue:BatchGetCrawlers",
                "glue:CreateCrawler",
                "glue:DeleteCrawler",
                "glue:GetCrawler",
                "glue:GetCrawlers",
                "glue:UpdateCrawler",
                "glue:StartCrawler",
                "glue:StopCrawler",
                "glue:GetCrawlerMetrics",
                "glue:GetCrawlerMetrics",
                "glue:CreateDevEndpoint",
                "glue:DeleteDevEndpoint",
                "glue:GetDevEndpoint",
                "glue:GetDevEndpoints",
                "glue:UpdateDevEndpoint",
                "glue:ListDevEndpoints",
                "glue:StartDevEndpoint",
                "glue:StopDevEndpoint",
                "glue:CreateJob",
                "glue:DeleteJob",
                "glue:GetJob",
                "glue:GetJobs",
                "glue:UpdateJob",
                "glue:StartJobRun",
                "glue:BatchStopJobRun",
                "glue:GetJobRun",
                "glue:GetJobRuns",
                "glue:GetUserDefinedFunction",
                "glue:GetUserDefinedFunctions",
                "glue:CreateSecurityConfiguration",
                "glue:DeleteSecurityConfiguration",
                "glue:GetSecurityConfiguration",
                "glue:GetSecurityConfigurations",
                "glue:UpdateSecurityConfiguration",
                "glue:BatchCreatePartition",
                "glue:BatchDeletePartition",
                "glue:CancelMLTaskRun",
                "glue:CreateMLTransform",
                "glue:DeleteMLTransform",
                "glue:GetMLTaskRun",
                "glue:GetMLTransform",
                "glue:GetMLTransforms",
                "glue:UpdateMLTransform",
                "glue:StartMLTransformRun",
                "glue:BatchGetTriggers",
                "glue:CreateTrigger",
                "glue:DeleteTrigger",
                "glue:GetTrigger",
                "glue:GetTriggers",
                "glue:UpdateTrigger",
                "glue:BatchCreateCrawler",
                "glue:BatchDeleteCrawler",
                "glue:BatchStopCrawler",
                "glue:GetCrawlerMetric",
                "glue:GetCrawlerMetrics",
                "glue:PutResourcePolicy",
                "glue:GetResourcePolicies",
                "glue:DeleteResourcePolicy",
                "glue:ListCrawlers",
                "glue:ListDevEndpoints",
                "glue:ListJobs",
                "glue:ListTriggers",
                "glue:ListWorkflows",
                "glue:GetWorkflow",
                "glue:GetWorkflowRuns",
                "glue:GetWorkflowRun",
                "glue:GetWorkflowGraph",
                "glue:StartWorkflowRun",
                "glue:StopWorkflowRun",
                "glue:BatchStopJobRun",
                "glue:GetJobRun",
                "glue:GetJobRuns",
                "glue:GetDataflowGraph",
                "glue:GetPlan",
                "glue:StartImportLabelsTaskRun",
                "glue:StartExportLabelsTaskRun",
                "glue:GetMLTaskRuns",
                "glue:GetMLTaskRun",
                "glue:CancelMLTaskRun",
                "glue:GetBlueprint",
                "glue:CreateBlueprint",
                "glue:ListBlueprints",
                "glue:ListBlueprintRuns",
                "glue:StartBlueprintRun",
                "glue:BatchGetBlueprints",
                "glue:DeleteBlueprint",
                "glue:ListRegistries",
                "glue:GetRegistry",
                "glue:CreateRegistry",
                "glue:UpdateRegistry",
                "glue:DeleteRegistry",
                "glue:GetResourcePolicy",
                "glue:BatchGetJobs",
                "glue:BatchGetPartition",
                "glue:BatchGetTable",
                "glue:GetColumnStatisticsForTable",
                "glue:GetPartition",
                "glue:GetTable",
                "glue:GetTables",
                "glue:UpdateColumnStatisticsForTable",
                "glue:GetPartitions",
                "glue:GetDatabase",
                "glue:GetDatabases",
                "glue:BatchCreatePartition",
                "glue:BatchDeletePartition",
                "glue:BatchGetPartition",
                "glue:BatchUpdatePartition",
                "glue:CreateDatabase",
                "glue:CreatePartition",
                "glue:CreateTable",
                "glue:DeletePartition",
                "glue:DeleteTable",
                "glue:DeleteTableVersion",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:GetTable",
                "glue:GetTableVersions",
                "glue:UpdateDatabase",
                "glue:UpdatePartition",
                "glue:UpdateTable",
                "glue:UpdateTableVersion",
                "glue:BatchCreatePartition",
                "glue:BatchDeletePartition",
                "glue:BatchGetPartition",
                "glue:BatchUpdatePartition",
                "glue:CreateDatabase",
                "glue:CreatePartition",
                "glue:CreateTable",
                "glue:DeletePartition",
                "glue:DeleteTable",
                "glue:DeleteTableVersion",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:GetTable",
                "glue:GetTableVersions",
                "glue:UpdateDatabase",
                "glue:UpdatePartition",
                "glue:UpdateTable",
                "glue:UpdateTableVersion",
                "glue:BatchCreatePartition",
                "glue:BatchDeletePartition",
                "glue:BatchGetPartition",
                "glue:BatchUpdatePartition",
                "glue:CreateDatabase",
                "glue:CreatePartition",
                "glue:CreateTable",
                "glue:DeletePartition",
                "glue:DeleteTable",
                "glue:DeleteTableVersion",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:GetTable",
                "glue:GetTableVersions",
                "glue:UpdateDatabase",
                "glue:UpdatePartition",
                "glue:UpdateTable",
                "glue:UpdateTableVersion",
                "glue:BatchCreatePartition",
                "glue:BatchDeletePartition",
                "glue:BatchGetPartition",
                "glue:BatchUpdatePartition",
                "glue:CreateDatabase",
                "glue:CreatePartition",
                "glue:CreateTable",
                "glue:DeletePartition",
                "glue:DeleteTable",
                "glue:DeleteTableVersion",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:GetTable",
                "glue:GetTableVersions",
                "glue:UpdateDatabase",
                "glue:UpdatePartition",
                "glue:UpdateTable",
                "glue:UpdateTableVersion",
                "glue:BatchCreatePartition",
                "glue:BatchDeletePartition",
                "glue:BatchGetPartition",
                "glue:BatchUpdatePartition"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "s3:ListAllMyBuckets"
            ],
            "Resource": [
                "arn:aws:s3:::your-source-bucket",
                "arn:aws:s3:::your-source-bucket/*",
                "arn:aws:s3:::your-target-bucket",
                "arn:aws:s3:::your-target-bucket/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateNetworkInterface",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DeleteNetworkInterface"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:region:account-id:log-group:/aws/glue/*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudwatch:PutMetricData",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:CreateServiceLinkedRole",
                "iam:PutRolePolicy",
                "iam:UpdateAssumeRolePolicy",
                "iam:PassRole",
                "iam:GetRole",
                "iam:CreateRole"
            ],
            "Resource": "*"
        }
    ]
}


```
Always follow the principle of least privilege, granting only the permissions necessary for each role to perform its intended tasks.
