# Turn Data Lake into Redshift Data warehousing

This is a lab guide to turn typical data lake architecture (S3 + Glue + Athena) into Redshift.
(The lab is an extension from AWS Data Engineering Immersion Day - Data Validation and ETL https://catalog.us-east-1.prod.workshops.aws/workshops/976050cc-0606-4b23-b49f-ca7b8ac4b153/en-US/600)

## 1. Create Redshift cluster

1. Login AWS Console
2. Go to Redshift portal - Provisioned clusters dashboard
3. Create Cluster
4. Select `Free Trial`
![image](https://user-images.githubusercontent.com/112601576/205628090-b41fa40c-5131-4ab8-9537-1d014e206549.png)
5. Use `awsuser` as the Admin user name
6. check `Auto generate password` checkbox
7. Wait 5-10 min for the Redshift cluster creation

## 2. Grant Glue Data Catalog permission for Redshift spectrum
1. Select the Redshift cluster
2. Action > Permission > Manage IAM role, copy the IAM role name.
3. Go to IAM portal in AWS console, select Role and find the IAM role from Step 2.
4. Add Managed IAM policy `AWSGlueConsoleFullAccess` into the IAM role. (In production, please use least privilage for the IAM role)
![image](https://user-images.githubusercontent.com/112601576/205630820-708a3db3-1b27-4363-80fc-daf4d2f02efd.png)


## 3. Query Editor
1. Select the Redshift cluster
2. Click `Query Data` > `Query data in query editor v2`
3. You will go into the web-based query editor
4. Select the redshift cluster > dev, then input the following SQL script into the query editor
```
CREATE external SCHEMA ticketdata
FROM data catalog DATABASE 'ticketdata'
IAM_ROLE default
CREATE external DATABASE if not exists;
```
5. `Run` to create a scheman ticketdata from Glue Data Catalog. This uses Redshift Spectrum service which queries the data from S3 + Glue data catalog, It is similar to Amazon Athena, but with dedictated resource in Redshift.
6. Try below query which is same as the one query from Athena in the workshop
```
SELECT
e.id AS event_id,
e.sport_type_name AS sport,
e.start_date_time AS event_date_time,
h.name AS home_team,
a.name AS away_team,
l.name AS location,
l.city
FROM ticketdata.parquet_sporting_event e,
ticketdata.parquet_sport_team h,
ticketdata.parquet_sport_team a,
ticketdata.parquet_sport_location l
WHERE
e.home_team_id = h.id
AND e.away_team_id = a.id
AND e.location_id = l.id;
```

## 4. Copy the data from data lake to Redshift storage

1. Create local database schema
```
Create schema local if not exists;
```
2. Create Table AS (CTAS) from Glue data query to local table
```
Create schema local if not exists;

create table local.sporting_event_full AS
SELECT
e.id AS event_id,
e.sport_type_name AS sport,
e.start_date_time AS event_date_time,
h.name AS home_team,
a.name AS away_team,
l.name AS location,
l.city
FROM ticketdata.parquet_sporting_event e,
ticketdata.parquet_sport_team h,
ticketdata.parquet_sport_team a,
ticketdata.parquet_sport_location l
WHERE
e.home_team_id = h.id
AND e.away_team_id = a.id
AND e.location_id = l.id;
```
3. local.sporting_event_full has been created with the data.
```
SELECT * FROM "dev"."local"."sporting_event_full" limit 10;

SELECT count(1) FROM "dev"."local"."sporting_event_full";
```

Migration completed.
