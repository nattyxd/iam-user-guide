# Amazon S3: Allows IAM users access to their S3 home directory, programmatically and in the console<a name="reference_policies_examples_s3_home-directory-console"></a>

This example shows how you might create an identity\-based policy that allows IAM users to access their own home directory bucket object in S3\. The home directory is a bucket that includes a `home` folder and folders for individual users\. This policy defines permissions for programmatic and console access\. To use this policy, replace the *italicized placeholder text* in the example policy with your own information\. Then, follow the directions in [create a policy](access_policies_create.md) or [edit a policy](access_policies_manage-edit.md)\.

This policy will not work when using IAM roles because the `aws:username` variable is not available when using IAM roles\. For details about principal key values, see [Principal key values](reference_policies_variables.md#principaltable)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::bucket-name",
            "Condition": {
                "StringLike": {
                    "s3:prefix": [
                        "",
                        "home/",
                        "home/${aws:username}/*"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::bucket-name/home/${aws:username}",
                "arn:aws:s3:::bucket-name/home/${aws:username}/*"
            ]
        }
    ]
}
```