# WATCHME
https://www.loom.com/share/67285644cf34404cbbe2880fb0e48d01

## Databricks free account
Part 0: Create a Databricks account
1.	Create a free Databricks account on https://databricks.com/try-databricks
2.	Press **Community Edition** on the next page.

## AWS Bucket Creation
Part 1: Create an s3 bucket
1.	Create/Open AWS account.
2.	Go to the **Service** drop down menu on the top left corner. Under **Storage** press **S3** then press **Create Bucket**.
3.	Name the bucket and make sure bucket is in correct region.
4.	Create bucket named something like `clusterchallenge`
5.	Press the name of the bucket you just created, press **Upload** then **Add files** and add your CSV.
6.	I recommend looking at Medicare spending data [by state](https://data.cms.gov/provider-data/dataset/rs6n-9qwg) and [per hospital](https://data.cms.gov/provider-data/dataset/5hk7-b79m), two datasets that can be treated relationally.

## AWS and Databricks Sync
Part 2:  Create an EC2 role to call AWS services on your behalf
1.	In the AWS console, go to the **Service** drop down menu on the top left corner. Under **Security, Identity & Compliance** press **IAM**.
2.  Click the **Roles** tab in the sidebar then click **Create role**.
3.  Under **Select type of trusted entity**, select **AWS service** then under **Choose a use case**, select **EC2**.
4.  In the bottom right corner click **Next: Permissions, Next: Tags, and Next: Review**.
5.  In the Role name field, type a role name like 'databricks' then click **Create role**.
6.  In the role list of roles displayed, click the role you just created.
7.  Under the **Permission** tab, click **Add Inline Policy** in the bottom right corner. This policy grants access to the S3 bucket.
8.  Click the **JSON** tab.
9.  Copy this policy and in 2 places within the policy replace *s3-bucket-name* with the name of your bucket.
10. When done click **Review policy**.
11. In the **Name** field, type a policy name like `ec2_perms` and click **Create policy**.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
     "Resource": [
        "arn:aws:s3:::<s3-bucket-name>"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject",
        "s3:PutObjectAcl"
      ],
      "Resource": [
         "arn:aws:s3:::<s3-bucket-name>/*"
      ]
    }
  ]
}
```
Part 3: Provide access to the objects stored in the bucket     
1.  In the AWS console go to **S3**.
2.  Press your bucket name and go to the **Permissions** tab.
3.  Scroll down to **Bucket Policy** and press **Edit**.
4.  Paste the following policy.
    - Replace *aws-account-id-databricks* with the AWS account ID where the Databricks environment is deployed. There are two places within the policy.
    - Replace *iam-role-for-s3-access* with the EC2 role you just created in Part 2. There are two places within the policy.
    - Replace *s3-bucket-name* with the bucket name. There are two places within the policy.
      - You can find your AWS Account ID by clicking on the drop down arrow on your User Name in the top right corner.
5.  Click **Save**.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Example permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<aws-account-id-databricks>:role/<iam-role-for-s3-access>"
      },
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::<s3-bucket-name>"
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<aws-account-id-databricks>:role/<iam-role-for-s3-access>"
      },
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject",
        "s3:PutObjectAcl"
      ],
      "Resource": "arn:aws:s3:::<s3-bucket-name>/*"
    }
  ]
}
```
Part 4: Create Cross Account Role
1.  Go back to Databricks and access the **Account Console** by clicking the user profile that looks like a figure in the top right corner.
2.  Select **Manage Account** and log in. Fill out details if required.
3.  Click the **AWS Account** side tab and select **Deploy to AWS using Cross Account Role**.
4.  In the **AWS Region** drop-down, select correct **AWS region**. Make sure it matches your the region in your bucket.
5.  Copy the **External ID**.
6.  In the AWS console, go to **IAM** then go to roles and click **Create role**.
7.  In **Select type of trusted entity**, click the **Another AWS** account tile.
8.  In the **Account ID** field, enter this Databricks hard coded account ID *414351767826* then select the **Require external ID** checkbox.
9.  In the **External ID** field, paste the Databricks External ID you copied earlier found under 'AWS Account'.
10. Click **Next: Permissions button**, **Next: Tags button** and **Next: Review button**.
11. In the Role name field, enter a role name and click **Create role**
12. The list of roles are displayed, click the role you just created.
13. Under the **Permissio**n tab, click **Add Inline Policy** in the bottom right corner.
14. In the policy editor, click the JSON tab and paste the following access policy into the editor, no changes are needed.
15. Click **Review policy**, enter a policy name and click **Create Policy**.
16. In **Summary**, copy the **Role ARN**.
 
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1403287045000",
      "Effect": "Allow",
      "Action": [
          "ec2:AssociateDhcpOptions",
          "ec2:AssociateIamInstanceProfile",
          "ec2:AssociateRouteTable",
          "ec2:AttachInternetGateway",
          "ec2:AttachVolume",
          "ec2:AuthorizeSecurityGroupEgress",
          "ec2:AuthorizeSecurityGroupIngress",
          "ec2:CancelSpotInstanceRequests",
          "ec2:CreateDhcpOptions",
          "ec2:CreateInternetGateway",
          "ec2:CreateKeyPair",
          "ec2:CreatePlacementGroup",
          "ec2:CreateRoute",
          "ec2:CreateSecurityGroup",
          "ec2:CreateSubnet",
          "ec2:CreateTags",
          "ec2:CreateVolume",
          "ec2:CreateVpc",
          "ec2:CreateVpcPeeringConnection",
          "ec2:DeleteInternetGateway",
          "ec2:DeleteKeyPair",
          "ec2:DeletePlacementGroup",
          "ec2:DeleteRoute",
          "ec2:DeleteRouteTable",
          "ec2:DeleteSecurityGroup",
          "ec2:DeleteSubnet",
          "ec2:DeleteTags",
          "ec2:DeleteVolume",
          "ec2:DeleteVpc",
          "ec2:DescribeAvailabilityZones",
          "ec2:DescribeIamInstanceProfileAssociations",
          "ec2:DescribeInstanceStatus",
          "ec2:DescribeInstances",
          "ec2:DescribePlacementGroups",
          "ec2:DescribePrefixLists",
          "ec2:DescribeReservedInstancesOfferings",
          "ec2:DescribeRouteTables",
          "ec2:DescribeSecurityGroups",
          "ec2:DescribeSpotInstanceRequests",
          "ec2:DescribeSpotPriceHistory",
          "ec2:DescribeSubnets",
          "ec2:DescribeVolumes",
          "ec2:DescribeVpcs",
          "ec2:DetachInternetGateway",
          "ec2:DisassociateIamInstanceProfile",
          "ec2:ModifyVpcAttribute",
          "ec2:ReplaceIamInstanceProfileAssociation",
          "ec2:RequestSpotInstances",
          "ec2:RevokeSecurityGroupEgress",
          "ec2:RevokeSecurityGroupIngress",
          "ec2:RunInstances",
          "ec2:TerminateInstances"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole",
        "iam:PutRolePolicy"
      ],
      "Resource": "arn:aws:iam::*:role/aws-service-role/spot.amazonaws.com/AWSServiceRoleForEC2Spot",
      "Condition": {
        "StringLike": {
          "iam:AWSServiceName": "spot.amazonaws.com"
        }
      }
    }
  ]
}
```
Part 5: After creating a second AWS IAM Role for extensive ec2 permissions, we return to Databricks to generate permissions for our S3 bucket.

1.  Return to the Databricks Account Console and in the **AWS Account** tab, paste the `databricks_cluster` **Role ARN** you copied in the last step into the **Role ARN** field.
2.  Click **Next Step**.
3.  Enter your bucket name and click **Generate Policy**. Copy the generated policy.
4.  In the AWS console, go to **S3** and click your bucket name.
5.  Go to the **Permissions** tab and scroll down to **Bucket Policy** then click **Edit**.
6.  Replace the policy with the copied generated policy from step 3 and click Save.
7.  Now go the **Properties** tab then the **Bucket Versioning** tile and click **Enable versioning** then **Save**.
8.  Go back to the Databricks Account Console and in the AWS Storage tab click **Apply Change**.
9.  Deployment should take about 30 minutes
  
Part 6: Add more permissions
1.	In the AWS console go to **IAM** then go to roles and click on the last role you created `databricks_cluster`.
2.  On the **Permissions** tab click the policy and click **Edit Policy** then press the JSON tab.
3.  Copy the following policy and replace the existing one. This will add one more permission.
    - Replace *iam-role-for-s3-access* with the first EC2 role you created.
    - Replace *aws-account-id-databricks* with your AWS account ID.
4.  Click **Review** policy and save changes.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1403287045000",
      "Effect": "Allow",
      "Action": [
        "ec2:AssociateDhcpOptions",
        "ec2:AssociateIamInstanceProfile",
        "ec2:AssociateRouteTable",
        "ec2:AttachInternetGateway",
        "ec2:AttachVolume",
        "ec2:AuthorizeSecurityGroupEgress",
        "ec2:AuthorizeSecurityGroupIngress",
        "ec2:CancelSpotInstanceRequests",
        "ec2:CreateDhcpOptions",
        "ec2:CreateInternetGateway",
        "ec2:CreateKeyPair",
        "ec2:CreatePlacementGroup",
        "ec2:CreateRoute",
        "ec2:CreateSecurityGroup",
        "ec2:CreateSubnet",
        "ec2:CreateTags",
        "ec2:CreateVolume",
        "ec2:CreateVpc",
        "ec2:CreateVpcPeeringConnection",
        "ec2:DeleteInternetGateway",
        "ec2:DeleteKeyPair",
        "ec2:DeletePlacementGroup",
        "ec2:DeleteRoute",
        "ec2:DeleteRouteTable",
        "ec2:DeleteSecurityGroup",
        "ec2:DeleteSubnet",
        "ec2:DeleteTags",
        "ec2:DeleteVolume",
        "ec2:DeleteVpc",
        "ec2:DescribeAvailabilityZones",
        "ec2:DescribeIamInstanceProfileAssociations",
        "ec2:DescribeInstanceStatus",
        "ec2:DescribeInstances",
        "ec2:DescribePlacementGroups",
        "ec2:DescribePrefixLists",
        "ec2:DescribeReservedInstancesOfferings",
        "ec2:DescribeRouteTables",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSpotInstanceRequests",
        "ec2:DescribeSpotPriceHistory",
        "ec2:DescribeSubnets",
        "ec2:DescribeVolumes",
        "ec2:DescribeVpcs",
        "ec2:DetachInternetGateway",
        "ec2:DisassociateIamInstanceProfile",
        "ec2:ModifyVpcAttribute",
        "ec2:ReplaceIamInstanceProfileAssociation",
        "ec2:RequestSpotInstances",
        "ec2:RevokeSecurityGroupEgress",
        "ec2:RevokeSecurityGroupIngress",
        "ec2:RunInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::<aws-account-id-databricks>:role/<iam-role-for-s3-access>"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole",
        "iam:PutRolePolicy"
      ],
      "Resource": "arn:aws:iam::*:role/aws-service-role/spot.amazonaws.com/AWSServiceRoleForEC2Spot",
      "Condition": {
        "StringLike": {
          "iam:AWSServiceName": "spot.amazonaws.com"
        }
      }
    }
  ]
}
```
Part 7: Create Instance Profile as they are more secure than access keys
1.  In the AWS console go to **IAM** and select Roles on the side menu.
2.  Go to the first EC2 role you created on IAM `databricks`, and copy **Instance Profile ARN** in the **Summary** section.
3.  Click on your Databricks cloud deployment link and access the 'Account Console' by clicking the user profile on the top right.
4.  Select **Admin Console** and click the **Instance Profiles** tab.
5.  Press **Add Instance Profile** and paste the **Instance Profile ARN** copied earlier then click **Add**.

Part 8: Create cluster
1. Go back to the main Databricks page.
2. Under **Common Tasks** click **New Cluster** and enter cluster name like `testcluster`.
3. Scroll down and drop down the arrow for the **Advanced Options** section.
4. On the **Instance** section, select your instance profile from the Instance Profile drop-down list then click **Create Cluster**.
5. Create your cluster with your preferred amount of workers.

# Databricks deployment ready

Part 9: Create notebook and verify access
1.	Go back to the main page and click **New Notebook** under **Common Tasks**.
2.  Name your notebook and set Default Language to Python. 
3.  Set cluster to the one you just created.
4.  To Verify that you can access the S3 bucket, use the following command and replace *s3-bucket-name* with your bucket name
      ```dbutils.fs.ls("s3a://<s3-bucket-name>/")```
      should return something like
      ```      
        Out[1]: [FileInfo(path='s3a://clusterchallenge/Medicare_Spending_Per_Beneficiary___Hospital.csv', name='Medicare_Spending_Per_Beneficiary___Hospital.csv', size=1135401),
        FileInfo(path='s3a://clusterchallenge/Medicare_Spending_Per_Beneficiary___State.csv', name='Medicare_Spending_Per_Beneficiary___State.csv', size=6469),
        FileInfo(path='s3a://clusterchallenge/oregon-prod/', name='oregon-prod/', size=0)]
    ```
    
5. Once verified, read the CSVs into a spark dataframe to begin analysis
