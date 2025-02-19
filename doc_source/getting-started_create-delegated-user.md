# Creating your first IAM delegated user and user group<a name="getting-started_create-delegated-user"></a>

To support multiple users in your AWS account, you must delegate permission to allow other people to perform only the actions you want to allow\. To do this, create an IAM user group with the permissions those people need and then add IAM users to the necessary user groups as you create them\. You can use this process to set up the user groups, users, and permissions for your entire AWS account\. 

This solution is best used by small and medium organizations where an AWS administrator can manually manage the users and user groups\. For large organizations, you can use [custom IAM roles](id_roles_providers_enable-console-custom-url.md), [federation](id_roles_providers.md), or [single sign\-on](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)\.

## Creating a delegated IAM user and user group \(console\)<a name="getting-started_create-admin-group-console"></a>

You can use the AWS Management Console to create an IAM user group with delegated permissions\. Then create an IAM user for someone else and add it to the user group\. 

**To create a delegated user group and user for someone else \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Policies**\. 

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab and on the right side of the window choose **Import managed policy**\.

1. In the **Import managed policies** window, type **power** to reduce the list of policies\. Then choose the **PowerUserAccess** row\.

1. Choose **Import** to display the policy in the **JSON** tab\.

1. Choose **Next: Tags** and then **Next: Review**\.

1. On the **Review policy** page, for **Name**, type **PowerUserExampleCorp**\. For **Description**, type **Allows full access to all services except those for user management**\. Then choose **Create policy** to save your work\.

1. In the navigation pane, choose **User groups** and then choose **Create group**\.

1. In the **User group name** box, type **PowerUsers**\.

1. In the list of policies, select the check box next to **PowerUserExampleCorp**\.

1. Choose **Create group**\.

1. In the navigation pane, choose **Users** and then choose **Add users**\.

1. For **User name**, type **mary\.major@examplecorp\.com**\.

1. Choose **Add another user** and type **diego\.ramirez@examplecorp\.com** for the second user\.

1. Select the check box next to **AWS Management Console access** and choose **Autogenerated password**\. By default, AWS forces the new user to create a new password when first signing in\. Select the check box next to **User must create a new password at next sign\-in**\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions** page, do not add permissions to the users\. You will add a policy after the user confirms that they have changed their password and signed in\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM resources](id_tags.md)\.

1. Choose **Next: Review** to see the list of user group memberships to be added to the new user\. When you are ready to proceed, choose **Create users**\.

1. Download or copy the passwords for your new users and deliver them to the users securely\. Separately, provide your users with a link to your IAM user console page and their user names\.

1. After your user confirms that they can successfully sign in, choose **Users** in the navigation pane, if necessary\. Then choose one of the user's names\.

1. In the **Permissions** tab, choose **Add permissions**\. Choose **Add user to group**, and then select the check box next to **PowerUsers**\.

1. Choose **Next: Review** and then **Add permissions**\. 

## Reducing the user group permissions<a name="getting-started_reduce-permissions"></a>

Members of the `PowerUser` user group have full access to all services except a few that provide user management actions \(like IAM and Organizations\)\. After a predefined period of inactivity \(such as 90 days\) has passed, you can review the services that your user group members have accessed\. Then you can reduce the permissions of the `PowerUserExampleCorp` policy to include only the services that your team needs\.

For more information about the last accessed information, see [Refining permissions in AWS using last accessed information](access_policies_access-advisor.md)\.

### Reviewing last accessed information<a name="getting-started_reduce-permissions-review"></a>

Wait for a predefined period of inactivity \(such as 90 days\) to pass\. Then review the last accessed information for your users or user groups to learn when they last attempted to access the services that your `PowerUserExampleCorp` policy allows\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **User groups** and then choose the **PowerUser** group name\.

1. On the user group summary page, choose the **Access Advisor** tab\. 

   The table of last accessed information shows when the user group members last attempted to access each service, in chronological order from the most recent attempt\. The table includes only the services that the policy allows\. In this case, the `PowerUserExampleCorp` policy allows access to all AWS services\. 

1. Review the table and make a list of the services that your user group members have recently accessed\.

   For example, assume that within the last month, your team has accessed only the Amazon EC2 and Amazon S3 services\. But six months ago, they accessed Amazon EC2 Auto Scaling and IAM\. You know that they were investigating EC2 Auto Scaling, but decided that it wasn't necessary\. You also know that they used IAM to create a role to allow Amazon EC2 to access data in an S3 bucket\. So you decide to scale back the user's permissions to allow access to only the Amazon EC2 and Amazon S3 services\.

### Editing a policy to reduce permissions<a name="getting-started_reduce-permissions-edit-policy"></a>

After you review your last accessed information, you can edit your policy to allow access to only the services that your users need\.

**To use data to allow access to only necessary services**

1. In the navigation pane, choose **Policies** and then choose the **PowerUserExampleCorp** policy name\.

1. Choose **Edit policy**, and then choose the **JSON** tab\. 

1. Edit the JSON policy document to allow only the services you want\.

   For example, edit the first statement that includes the `Allow` effect and the `NotAction` element to allow only Amazon EC2 and Amazon S3 actions\. To do this, replace it with the statement with the `FullAccessToSomeServices` ID\. Your new policy will look like the following example policy\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
         {
             "Sid": "FullAccessToSomeServices",
             "Effect": "Allow",
             "Action": [
                 "ec2:*",
                 "s3:*"
             ],
             "Resource": "*"
         },
         {
             "Effect": "Allow",
             "Action": [
                 "iam:CreateServiceLinkedRole",
                 "iam:DeleteServiceLinkedRole",
                 "iam:ListRoles",
                 "organizations:DescribeOrganization"
             ],
             "Resource": "*"
         }
     ]
   }
   ```

1. To support the best practice of [granting least privilege](best-practices.md#grant-least-privilege), review and correct any errors, warnings, or suggestions returned during [policy validation](access_policies_policy-validator.md)\.

1. To further reduce your policies' permissions to specific actions and resources, view your events in CloudTrail **Event history**\. There you can view detailed information about the specific actions and resources that your user has accessed\. For more information, see [Viewing CloudTrail Events in the CloudTrail Console](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events-console.html) in the *AWS CloudTrail User Guide*\.