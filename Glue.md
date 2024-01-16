## Glue PySpark Scripting :
  * AWS Glue is a fully managed extract, transform, and load (ETL) service that makes it easy for you to prepare and load your data for analysis. PySpark is the 
    Python API for Apache Spark, a fast and general-purpose cluster computing system.
    Below are the Steps to create a Glue job

  * Step 1: Create a Glue Job:
            Go to the AWS Glue Console.
            Click on "ETL Jobs" in the left navigation pane.
            Click "Script Editor".
            Select Engine "Spark", Options: "Start Fresh" and Click Create Script.
            In the "This job runs" section, select "A new script to be authored by you."
            Click "Next" and configure the data source and target.
    
   ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/60fbe141-26fb-4294-88dc-31d0a927e0db)


 * Glue Script, Below is the glue script which is used to move the file from one S3 bucket to another S3 bucket.
   
   ![image](https://github.com/jaykumsi/aws-glue/assets/137452836/9aaed6e1-4f4c-462e-8ea4-4c9863264450)

 * 
