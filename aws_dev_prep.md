# AWS Developer prep-logs
Q | Info 
--- | ---
AWS OpsWorks| A configuration management service that enables you to configure and operate applications using Chief
AWS CloudFormation | A building block service that enables customers to provision and manage almost any AWS resource via a JSON-based domain specific language.
AWS Elastic Beanstalk | An application mamagement service
EMR slave task node|only runs tasks that does not store data
slave core nodes|You cannot remove them because they hold data
Lambda function|Stores your code on Amazon S3
Multi-AZ Db standby| Using __synch__ replication, not __Asynch__
Security Groups| Assigned only to instances in __region__ (not globally).
IAM Role Access Keys| Auto rotated by AWS, no need to change them
ELB SSL Termination|Terminates security of connection over HTTP
CloudFormation | Requires no polling instance. Make an S3 notification configuration which publishes to AWS Lambda on the manifest bucket. Make the Lambda create a CloudFormation Stack which contains the logic to construct an autoscaling worker tier of EC2 G2 instances with the ANN code on each instance. Create an SQS queue of the images in the manifest. Tear the stack down when the queue is empty.
Deploy|AWS recommends Blue-Green for zero-downtime deploys.
Deploy Dynamo DB|Use CloudFront. Neither AWS OpsWorks nor AWS Elastic Beanstalk directly supports DynamoDB
third parties|When third parties require access to your organization's AWS resources, you can use roles to delegate access to them.
