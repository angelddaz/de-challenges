## Databricks 
Part 0:
1.	Create a free Databricks account on https://databricks.com/try-databricks
2.	Press **Community Edition** on the next page.

AWS
Part 1:
1.	Create/Open AWS account.
2.	Go to the **Service** drop down menu on the top left corner. Under **Storage** press **S3**.
3.	Press **Create Bucket**.
1.	Name the bucket (make sure bucket is in correct time zone/region)
2.	Create bucket.
4.	Press on your bucket name.
5.	Press **Upload** then Add files and add your CSV.

# AWS and Databricks Sync

Part 2: 
1.	In the AWS console, go to the **Service** drop down menu on the top left corner. Under **Security, Identity & Compliance** go to **IAM**.
    - Click the **Roles** tab in the sidebar.
    - Click Create role.
    - Under **Select type of trusted entity**, select **AWS service**.
    - Under Choose the service that will use this role, select **EC2**.
    - In the bottom right corner click **Next: Permissions, Next: Tags, and Next: Review**.
    - In the Role name field, type a role name.
    - Click Create role. The list of roles displays.
    - In the role list, click the role you just created.
    - Under the **Permission** tab, click **Add Inline Policy** in the bottom right corner. This policy grants access to the S3 bucket.
    - Click the JSON tab.
    - Copy this policy and set *s3-bucket-name* to the name of your bucket:

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
    
- Click **Review policy**.
    - In the **Name** field, type a policy name.
    - Click **Create policy**.
    - Scroll up, in the **Summary** sections, copy the **Instance Profile ARN**.
2. In the AWS console, go to the **Service** drop down menu on the top left corner. Under **Storage** go to **S3**.
    - Press your bucket name and go to the **Permissions** tab
    - Scroll down to **Bucket Policy** and press **Edit**
    - Paste the following policy, replacing *aws-account-id-databricks* with the AWS account ID where the Databricks environment is deployed. Replace *iam-role-for-s3-access* with the role you created in Step 1, and *s3-bucket-name* with the bucket name.
      - You can find your AWS Account ID by clicking on the drop down arrow on your User Name in the top right corner.

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

   - Click Save.
   
3.  Go back to Databricks and access the **Account Console** by clicking the user profile that looks like a figure.
    - Select **Manage Account** and log in.
    - Click the **AWS Account** tab.
    - Select the **Deploy to AWS using Cross Account Role** radio button.
    - In the **AWS Region** drop-down, select correct **AWS region**. Make sure it matches your the region in your bucket.
    - Copy the **External ID**.
    - In the AWS console, go to the **Service** drop down menu on the top left corner. Under **Security, Identity & Compliance** go to **IAM**.
    - Click the Roles tab in the sidebar.
    - Click Create role.
    - In **Select type of trusted entity**, click the **Another AWS** account tile.
    - In the **Account ID** field, enter the Databricks account ID *414351767826*.
    - Select the **Require external ID** checkbox.
    - In the **External ID** field, paste the Databricks External ID you copied earlier.
      - To find External ID, go back to Databricks and access the Account Console by clicking the user profile that looks like a figure. Select Manage Account and log in.
    - Click the **Next: Permissions button**.
    - Click the **Next: Tags button**.
    - Click the **Next: Review button**.
    - In the Role name field, enter a role name.
    - Click Create role. The list of roles displays.
    - In the list of roles, click the role you created.
    - Under the **Permissio**n tab, click **Add Inline Policy** in the bottom right corner.
    - In the policy editor, click the JSON tab.
    - Paste this access policy into the editor:
 
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

- Click **Review policy**.
  - In the Name field, enter a policy name
  - Click **Create policy**.
  - In **Summary**, copy the **Role ARN**.
  - In the Databricks Account Console, return to the **AWS Account** tab and in the **Role ARN** field, paste the **Role ARN** you copied in the last step.
  - Click **Next Step**.
  - Enter your bucket name and click **Generate Policy**.
  - Copy the generated policy.
  - In the AWS console, go to the **Service** drop down menu on the top left corner. Under **Storage** go to **S3**.
  - Click the bucket name.
  - Click the **Permissions** tab.
  - Click the **Bucket Policy** button and click **Edit**.
  - Replace the policy that you copied in Step 1 and click Save.
  - Click the **Properties** tab.
  - Click the **Versioning** tile.
  - Click **Enable versioning** and click **Save**.
  - Go back to the Databricks Account Console, go to the AWS Storage tab.
  - Click Apply Change.
  - Deploy and it should take about 30 minutes
  
4.	In the AWS console, go to the **Service** drop down menu on the top left corner. Under Security, Identity & Compliance go to IAM.
    - Click the **Roles** tab in the sidebar.
    - Click on the last role created.
    - On the **Permissions** tab, click the policy.
    - Click **Edit Policy**, go to the JSON tab
    - Copy this policy

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

   - Replace the policy.
     - Replace *iam-role-for-s3-access* with the first role you created.
     - Replace *aws-account-id-databricks* with your AWS account ID.
     - Click **Review** policy.
     - Click Save changes.
    
5.  In the AWS console, go to the **Service** drop down menu on the top left corner. Under **Security, Identity & Compliance** go to **IAM**.
    - Select Roles on the sidebar menu
    - Go to the first role you created on IAM, and copy **Instance Profile ARN** in the **Summary** section.
    - Go back to Databricks and access the Account Console by clicking the user profile that looks like a figure.
    - Select **Admin Console**.
    - Click the **Instance Profiles** tab.
      - If you don’t see Instance Profile tab, you may need to open Databricks in a new window and through the community edition.
    - Paste **Instance Profile ARN**.
    - Click Add.
6.	Go back to the main Databricks page
    - Under **Common Tasks** click **New Cluster**.
    - Enter cluster name.
    - Scroll down and drop down arrow for the Advanced Options section.
    - On the Instances tab, select the instance profile from the Instance Profile drop-down list.
    - Click **Create Cluster**.

# Databricks deployment ready

Part 3:
1.	Go back to the main page and click **New Notebook** under **Common Tasks**.
    - Name your notebook and set Default Language to Python. 
    - Set cluster to the one you just created.
    - To Verify that you can access the S3 bucket, use the following command and replace *s3-bucket-name* with your bucket name
      ```dbutils.fs.ls("s3a://<s3-bucket-name>/")```
    - Once verified, read in the csv into a df
