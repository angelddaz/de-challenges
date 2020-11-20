## Databricks 

Part 0:
1. Create a free Databricks account on https://databricks.com/try-databricks  
  - Press community edition on the next page  
2. Go to email and reset password  
3. Create notebook  
  - Name: sparkchallenge  
  - Set Default Language to Python  

## AWS 
Part 1:
1. Create/Open AWS account  
2. Go to s3 buckets  
3. Create a new bucket called spark challenge  
  - Enable Bucket versioning  
4. Upload csv  

## AWS and Databricks Sync
Part 2:
1. Follow steps 1-6 here: https://docs.databricks.com/administration-guide/cloud-configurations/aws/instance-profiles.html  
  - In step 1 part 3d: Name your role airflow_orchestrator_role  
  - In step 1 part 5e: Name your policy AWS_ELT_Policy  
  - In step 2 part 1 you can find your AWS Account ID by following steps here: https://www.apn-portal.com/knowledgebase/articles/FAQ/Where-Can-I-Find-My-AWS-Account-ID#:~:text=Your%20AWS%20ID%20is%20the,underneath%20the%20Account%20Settings%20section.  
  - If problems arise in step 3, follow steps 1-3 of "Use a cross-account role" ONLY: https://docs.databricks.com/administration-guide/account-settings/aws-accounts.html  
  - In step 4 go to the role generated in step 3  
    * replace the policy  
      * replace iam-role-for-s3-access with the role you created in Step 1  
      * replace aws-account-id-databricks with your AWS account ID  
  - In step 6 create a new cluster  

## Databricks deployment ready
Part 3:
1. Go back to your notebook on databricks  
2. Run it in the cluster you just created with the instance profile  
3. Read in the csv into a df  
