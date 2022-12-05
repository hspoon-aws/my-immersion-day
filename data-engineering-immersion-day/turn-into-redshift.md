# Turn Data Lake into Redshift Data warehousing

This is a lab guide to turn typical data lake architecture (S3 + Glue + Athena) into Redshift.
(The lab is an extension from AWS Data Engineering Immersion Day)

## 1. Create Redshift cluster

1. Login AWS Console
2. Go to Redshift portal - Provisioned clusters dashboard
3. Create Cluster
4. Select `Free Trial`
![image](https://user-images.githubusercontent.com/112601576/205628090-b41fa40c-5131-4ab8-9537-1d014e206549.png)
5. Use `awsuser` as the Admin user name
6. check `Auto generate password` checkbox
7. Wait 5-10 min for the Redshift cluster creation


## 2. Query Editor

1. Select the Redshift cluster
2. Action > Permission > Manage IAM role, then add 
3. Click `Query Data` > `Query data in query editor v2`
4. You will go into the web-based query editor
5. Select the redshift cluster > dev, then input the following SQL script into the query editor
```
CREATE external SCHEMA ticketdata
FROM data catalog DATABASE 'ticketdata'
IAM_ROLE default
CREATE external DATABASE if not exists;
```
5. `Run` to create a scheman ticketdata from Glue Data Catalog. This uses Redshift Spectrum service which queries the data from S3 + Glue data catalog, It is similar to Amazon Athena, but with dedictated resource in Redshift.
6. 
