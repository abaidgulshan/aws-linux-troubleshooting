# aws-linux-troubleshooting

## How to combine two files line by line in linux bash script
* `paste file1.txt file2.txt > fileresults.txt`

## Add prefix and suffix to every line in a file bash script

* `awk '{ print "Prefix TXT", $0, "Suffix TXT" }' file.txt`

## PHP output showing little black diamonds with a question mark
php.ini and add:

* `default_charset = "ISO-8859-1"`
## Docker overlays disk space 
* `docker system prune -a -f`

## Install MYSQL connector ubuntu 16.04 for tableau server
* `https://www.tableau.com/support/drivers?_ga=2.162700796.1335875126.1612552598-743759460.1612552598&_fsi=Dr99RyGZ`

## Error remove long listed files /bin/rm: Argument list too long
* `find . -name "*.pdf" -print0 | xargs -0 rm`

## Error git Jenkins unable to connect to github private repo
* Try to connect to github private repo in Jenkins 
* issue: Its sending error `failed to connect to repository : error performing git command: git ls-remote -h https://github.com/`
* Solution: As in I have installed Jenkins on Amazon Linux make sure you install git on it , `yum install git -y`
* reference: https://stackoverflow.com/questions/76536785/jenkins-unable-to-connect-to-github-private-repo

## Decode file Base64
* Try to upload base64 encoded file to AWS secret manager
* Solution: `base64 -w 0 AWS-ClientVPN.xml`

## Encrypt AWS CloudTrail logs Cross account 
* 🤔  **Try**: I have two Accounts a master account and a sub account that is used for logging. My goal is to send the CloudTrail logs from the master account to the s3 bucket in the logging account. At this point I have configured the the CloudTrail logs to point to the s3 bucket in the logging account
* ❌ **Error**: I am getting the error `You don't have adequate permissions in S3 to perform this operation`.
* 🎯 **Solution**: Steps
  * created the customer-managed KMS on the same account + region as the s3.
  * to the KMS, added following policy
    ```
     {
       "Sid": "Allow use of the key",
       "Effect": "Allow",
       "Principal": {
           "Service": "cloudtrail.amazonaws.com"
        },
        "Action": "kms:*",     # "kms:ReEncryptFrom", "kms:Decrypt" should be enough though
        "Resource": "KMS_KEY_ARN"
      }
    ```
  * enable defatul-encryption on the S3 bucket, selecting created KMS
  * in the cloudtrail (org managed account), provide the KMS with the full KMS_KEY_ARN, not just the name
* 🙏🏻 **Reference**: [Steps](https://stackoverflow.com/questions/69740814/troubleshooting-kms-key-policies-for-cross-account-decryption)https://stackoverflow.com/questions/69740814/troubleshooting-kms-key-policies-for-cross-account-decryption

## Jenkinsfile Docker push issue
* 🤔  **Try**: Try to create docker image and push to ECR
* ❌ **Error**: I am getting the error `End of Pipeline groovy.lang.MissingPropertyException: No such property: docker for class: groovy.lang.Binding`.
* 🎯 **Solution**: The issue was I needed to install the Docker Pipeline plugin in Jenkins.

    * To install the plugin using the GUI:

    * `Dashboard > Manage Jenkins > Manage Plugins > Available (tab) > docker-pipeline.`
* 🙏🏻 **Reference**: https://stackoverflow.com/questions/41215997/jenkins-error-no-such-property-docker-for-class-groovy-lang-binding

## Jenkinsfile Docker push ECR issue
* 🤔  **Try**: Try to create docker image and push to ECR
* ❌ **Error**: I am getting the error `ERROR: Could not find credentials matching ecr:us-east-1:awscreds`.
* 🎯 **Solution**: The issue was I needed to install the Amazon ECR plugin in Jenkins.

    * To install the plugin using the GUI:

    * `Dashboard > Manage Jenkins > Manage Plugins > Available (tab) > Amazon ECR.`
* 🙏🏻 **Reference**: https://stackoverflow.com/questions/47937146/is-it-possible-to-provide-my-aws-credentials-in-the-docker-withregistry-call-in

## IAM billing access allow 
* 🤔  **Try**: Try to add billing permission for IAM user 
* ❌ **Error**: IAM AWS user unable to access billing console still
* 🎯 **Solution**:
  * Sign in to the AWS Management Console with your AWS account root user credentials.
  * In the navigation bar, choose your account name, and then choose Account.
  * Next to IAM user and Role Access to Billing Information, choose Edit.
  * Select the Activate IAM Access check box to activate access to the Billing and Cost Management console pages.
  * Note: Because this setting is deactivated by default, the root user must manually activate it. For more information on activating this setting, see Granting access to your billing information and tools.
  * Choose Update.
* 🙏🏻 **Reference**: https://repost.aws/knowledge-center/iam-billing-access#
![image](https://github.com/abaidgulshan/aws-linux-troubleshooting/assets/7329596/c747339c-3782-4283-a3a8-868e2e3efee0)

## ECR Public repo 403 Forbidden
* 🤔  **Try**: Try to install Karpenter helm chart to AWS EKS
* ❌ **Error**: `Error: unexpected status from HEAD request to https://public.ecr.aws/v2/karpenter/karpenter/manifests/0.35.0: 403 Forbidden`
* 🎯 **Solution**: Logout of helm registry to perform an unauthenticated pull against the public ECR
  * `helm registry logout public.ecr.aws`

## You are not authorized to perform this operation
* 🤔  **Try**: Try to install Karpenter Nodes with NodePools configuration
* ❌ **Error**: `You are not authorized to perform this operation. Encoded authorization failure message: KDmmJmkLKmQhatEqYt...MN3iUtfAa`
* 🎯 **Solution**: use the following aws cli command from the console
  * `aws sts decode-authorization-message --encoded-message KDmmJmkLKm...iUtfAa`

## SCP policy for single bucket
* 🤔  **Try**: Try to apply SCP policy for single S3 bucket
* 🎯 **Solution**: use the following aws cli command from the console
    ```
     {
         "Version": “2012–10–17”,
         "Statement": [
             {
                 "Sid": “s3-versioning-and-s3-object-delete-restriction”,
                 "Effect": "Deny",
                 "Action": [
                     "“s3": "DeleteObjectVersion”",
                     "“s3": "PutBucketVersioning”",
                     "“s3": "PutObjectVersionAcl”"
                 ],
                 "Resource": [
                     "“arn": "aws": "s3": : : scpolicyfors3bucket”
                 ]
             }
         ]
     }
   ```
## vim add extra character end of each line
* 🤔  **Try**: add extra character end of each line
* 🎯 **Solution**: `:%s/$/\*/g`
* 🙏🏻 **Reference**: https://stackoverflow.com/questions/594448/how-can-i-add-a-string-to-the-end-of-each-line-in-vim
