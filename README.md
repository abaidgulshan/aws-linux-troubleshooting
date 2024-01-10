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
* ❌ **Error**: I get the error `You don't have adequate permissions in S3 to perform this operation`.
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
