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
  # JDBC 
      * Amazon Redshift
      * Snowflake
      * Amazon Aurora
      * MariaDB
      * Microsoft SQL Server
      * MySQL
      * Oracle
      * PostgreSQL
  # MangoDB client 
      * Mongo DB
      * MongoDB Atlas
      * Amazon DocumentDB

    
