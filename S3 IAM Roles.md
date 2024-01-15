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
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::tini-d-gluebucket-001*"
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





