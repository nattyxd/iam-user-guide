# Using multi\-factor authentication \(MFA\) in AWS<a name="id_credentials_mfa"></a>

For increased security, we recommend that you configure multi\-factor authentication \(MFA\) to help protect your AWS resources\. You can enable MFA for IAM users or the AWS account root user\. When you enable MFA for the root user, it affects only the root user credentials\. IAM users in the account are distinct identities with their own credentials, and each identity has its own MFA configuration\.

**Topics**
+ [What is MFA?](#id_credentials_mfa-what-is-mfa)
+ [Enabling MFA devices for users in AWS](id_credentials_mfa_enable.md)
+ [Checking MFA status](id_credentials_mfa_checking-status.md)
+ [Resynchronizing virtual and hardware MFA devices](id_credentials_mfa_sync.md)
+ [Deactivating MFA devices](id_credentials_mfa_disable.md)
+ [What if an MFA device is lost or stops working?](id_credentials_mfa_lost-or-broken.md)
+ [Configuring MFA\-protected API access](id_credentials_mfa_configure-api-require.md)
+ [Sample code: Requesting credentials with multi\-factor authentication](id_credentials_mfa_sample-code.md)

## What is MFA?<a name="id_credentials_mfa-what-is-mfa"></a>

MFA adds extra security because it requires users to provide unique authentication from an AWS supported MFA mechanism in addition to their regular sign\-in credentials when they access AWS websites or services: 
+ **Virtual MFA devices**\. A software app that runs on a phone or other device and emulates a physical device\. The device generates a six\-digit numeric code based upon a time\-synchronized one\-time password algorithm\. The user must type a valid code from the device on a second webpage during sign\-in\. Each virtual MFA device assigned to a user must be unique\. A user cannot type a code from another user's virtual MFA device to authenticate\. Because they can run on unsecured mobile devices, virtual MFA might not provide the same level of security as U2F devices or hardware MFA devices\. We do recommend that you use a virtual MFA device while waiting for hardware purchase approval or while you wait for your hardware to arrive\. For a list of a few supported apps that you can use as virtual MFA devices, see [Multi\-Factor Authentication](http://aws.amazon.com/iam/details/mfa/)\. For instructions on setting up a virtual MFA device with AWS, see [Enabling a virtual multi\-factor authentication \(MFA\) device \(console\)](id_credentials_mfa_enable_virtual.md)\.
+ **U2F security key**\. A device that you plug into a USB port on your computer\. U2F is an open authentication standard hosted by the [FIDO Alliance](https://fidoalliance.org)\. When you enable a U2F security key, you sign in by entering your credentials and then tapping the device instead of manually entering a code\. For information on supported AWS U2F security keys, see [Multi\-Factor Authentication](http://aws.amazon.com/iam/details/mfa/)\. For instructions on setting up a U2F security key with AWS, see [Enabling a U2F security key \(console\)](id_credentials_mfa_enable_u2f.md)\. 
+ **Hardware MFA device**\. A hardware device that generates a six\-digit numeric code based upon a time\-synchronized one\-time password algorithm\. The user must type a valid code from the device on a second webpage during sign\-in\. Each MFA device assigned to a user must be unique\. A user cannot type a code from another user's device to be authenticated\. For information on supported hardware MFA devices, see [Multi\-Factor Authentication](http://aws.amazon.com/iam/details/mfa/)\. For instructions on setting up a hardware MFA device with AWS, see [Enabling a hardware MFA device \(console\)](id_credentials_mfa_enable_physical.md)\.
**Note**  
**SMS text message\-based MFA**\. AWS ended support for enabling SMS multi\-factor authentication \(MFA\)\. We recommend that customers who have IAM users that use SMS text message\-based MFA switch to one of the following alternative methods: [virtual \(software\-based\) MFA device](id_credentials_mfa_enable_virtual.md), [U2F security key](id_credentials_mfa_enable_u2f.md), or [hardware MFA device](id_credentials_mfa_enable_physical.md)\. You can identify the users in your account with an assigned SMS MFA device\. To do so, go to the IAM console, choose **Users** from the navigation pane, and look for users with **SMS** in the **MFA** column of the table\.

For answers to commonly asked questions about AWS MFA, go to the [AWS Multi\-factor authentication FAQs](http://aws.amazon.com/iam/faqs/#Multi-factor_authentication)\. 