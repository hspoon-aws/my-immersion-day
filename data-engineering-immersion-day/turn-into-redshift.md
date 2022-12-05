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
