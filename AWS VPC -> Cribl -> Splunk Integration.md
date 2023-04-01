**To integrate AWS VPC Flow logs with Crible to splunk cloud.**

**AWS Pre request**
	To integrate AWS logs with Cribl we need S3 bucket, SQS.
  
**Pre-requisite**

1.Create a IAM user with full Permission Policy of AWS S3 Bucket, VPC, SQS.
2.Create S3 Bucket to store the logs.
3.Create SQS to send logs to the Crible data source.

**IAM User**
To create a IAM user
1.From the AWS Console, Search  IAM from the search bar.
2.Create a IAM user with Access key - Programmatic access.

![Picture1](https://user-images.githubusercontent.com/125336591/229310857-fa07ad33-479c-448a-a6fa-ff5279a5b686.png)

3.Attach the Existing Policies of AmazonS3FullAccess, AmazonSQSFullAccess, AWSCloudTrail Full Access with this user.

![Picture2](https://user-images.githubusercontent.com/125336591/229310882-69016cd0-4a22-4a2c-a4d8-d7e566f00173.png)

4.Save the user Security credential for the feature Purpose.
Note: Download the security credentials csv file.

![Picture3](https://user-images.githubusercontent.com/125336591/229310901-a0a44ded-2295-4204-8a94-8f404a9a02ef.png)

**SQS**

To create a SQS,From the AWS Console, Search  SQS from the search bar.
1.In SQS service click create Queue
Step1: Create queue
	Type: choose Standard
	Name: queue name
	Encryption: Server-side encryption Disabled
	Access policy: Basic
1.After choosing all the parameters, click on the create queue button.
2.After the Queue is created select the Queue that you created.
3.Select the Access policy and click Edit.
4.Edit the policy to allow SQS Queue to collect the message from S3 and send.


Example JSON policy
{
    "Version": "2012-10-17",
    "Id": "example-ID",
    "Statement": [
        {
            "Sid": "example-statement-ID",
            "Effect": "Allow",

            "Principal": {
                "Service": "s3.amazonaws.com"
            },
            "Action": [
                "SQS:SendMessage"
            ],
            "Resource": "arn:aws:sqs:Region:account-id:queue-name",
            "Condition": {
                "ArnLike": {
                    "aws:SourceArn": "arn:aws:s3:*:*:awsexamplebucket1"
                },
                "StringEquals": {
                    "aws:SourceAccount": "bucket-owner-account-id"
                }
            }
        }
    ]
}


**S3 Bucket**

1.From the AWS Console, Search  S3 from the search bar.
2.Select the S3 Bucket containing the VPC Flow logs.
3.Select the tab Properties of the S3 Bucket.
4.After entering all the parameters, click on the Save changes button.
5.In the Event notifications click create Event notification.
    General configuration
    Event name: Enter the custom event name
    Event types
    Object creation: choose All object create events
    Destination
    Choose SQS Queue
    Specify SQS queue: Enter SQS queue ARN

6.After entering all the parameters, click on the Save changes button.

**VPC**
1.From the AWS Console, Search  VPC from the search bar.
2.Select the VCP to Monitor the VPC flow logs.
3.select Flow logs and click Create flow log.
Flow log settings
Name: Custom Flow log name
Filter: ALL
Maximum aggregation interval: 10 minutes
Destination: Send to an Amazon S3 bucket
	S3 bucket ARN: Enter the S3 bucket ARN to store VPC Flow logs


**Cribl**
Cribl Data source
1.login to cribl Cloud Environment.
2.In-stream select  manage Stream.
3.Select the Default worker groups.
4.In that worker group select Data  sources
5.Choose Amazon S3 Data source
**General Settings:**
	Input ID: Enter a Custom name to identify this S3 Source definition.
	Queue:  ARN of the SQS queue to read events from S3
** Authentication:**
 Choose the manual Authentication Method
	Access key: Enter the AWS access key
	Secret key: Enter the AWS secret key
** Event Breakers:**
 Add ruleset : AWS Ruleset Event breaking rules for common AWS data sources (5 rules)
** Fields:**
 Add Fields: index and Source type
 **Connected Destinations:** send to routes
6.After entering all the parameters, click on the Save changes button.


**General Settings**

![Picture4](https://user-images.githubusercontent.com/125336591/229311044-ffa30a2d-8e93-4b66-82db-8e8552ed0ab6.png)

**Authentication**

![Picture5](https://user-images.githubusercontent.com/125336591/229311051-49ea2dd8-8065-4bea-812c-89cc959be79d.png)


**EVENT BREAKERS**


![Picture6](https://user-images.githubusercontent.com/125336591/229311089-5495db58-9aca-421d-ac59-f0da4a0c3fe0.png)

**Fields**

![Picture7](https://user-images.githubusercontent.com/125336591/229311092-3b6db98d-2349-4ce9-beeb-55703358a864.png)

**Connected Destinations**

![Picture8](https://user-images.githubusercontent.com/125336591/229311152-a42bda0a-e275-43f0-adbe-f23ff20069c2.png)

**Cribl Destination******

**Destination:**
1.Data  Destinations  choose Splunk instance.
2.Login to splunk machine add the receiving port and add the index that you mention in the data source in the cribl. 
**Routing:**
3.Routing  Data routing
4.Add new routes
Route Name: Enter the custom Route Name
Filter: Select the Source filter
Pipeline: passthru
Output: Choose the Splunk cloud
5.After all, above steps to be created click Commit and deploy on the top right corner.



![Picture9](https://user-images.githubusercontent.com/125336591/229311189-6a31ea30-bcb6-4c34-90e6-ad2074dc44c5.png)

