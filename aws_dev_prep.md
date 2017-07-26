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
AWS API Gateway|by default throttles at 500 requests per second steady-state
Lambda| by default, throttles at 100 concurrent requests for safety.
Use 9001 MTU instead of 1500 for Jumbo Frames, to raise packet body to packet overhead ratios.
UDP large file|	Use 9001 MTU instead of 1500 for Jumbo Frames, to raise packet body to packet overhead ratios. https://stackoverflow.com/questions/14010635/how-to-find-mtu-value-of-network-through-codein-python
CloudFormation stack status pooling|ListStacks / list-stacks
OpsWorks|The stack is the core AWS OpsWorks Stacks component. 
AWS Cognito |Build the application out using AWS Cognito and web identity federation to allow users to log in using Facebook or Google Accounts. Once they are logged in, the secret token passed to that user is used to directly access resources on AWS, like AWS S3. https://aws.amazon.com/blogs/security/how-does-amazon-cognito-relate-to-existing-web-identity-federation/
identity provider| Login with Amazon, Facebook, Google, or any other OpenID Connect (OIDC)-compatible IdP, receive an authentication token, and then exchange that token for temporary security credentials in AWS that map to an IAM role with permissions to use the resources in your AWS account.
Large ETL JOb|o declaratively model build and destroy of a cluster, you need to use AWS CloudFormation. 
CloudFormation|Putting all resources in one stack is a bad idea, since different tiers have different life cycles and frequencies of change. you can use two common frameworks: a multi-layered architecture and service-oriented architecture (SOA).
Layered Stack deployment|Only CloudFormation allows source controlled, declarative templates as the basis for stack automation. Nested Stacks help achieve clean separation of layers while simultaneously providing a method to control all layers at once when needed.
way to track expenses|Cost Allocation Tagging is a built-in feature of AWS, and when coupled with the Cost Explorer, provides a simple and robust way to track expenses.
Realtime access recon|CloudWatch Events allow subscription to AWS API calls, and direction of these events into Kinesis Streams. This allows a unified, near real-time stream for all API calls, which can be analyzed with any tool(s) of your choosing downstream.
Caching queries|CloudFront cannot directly cache DynamoDB queries,  use Elasti Cache instead
Resource unsupported by CloudFormation|Create a CloudFormation Custom Resource Type by implementing create, update, and delete functionality, either by subscribing a Custom Resource Provider to an SNS topic, or by implementing the logic in AWS Lambda.
Elasti Beanstalk|Applications group logical services. Environments belong to Applications, and typically represent different deployment levels (dev, stage, prod, fo forth). Deployments belong to environments, and are pushes of bundles of code for the environments to run.
Elastic Beanstalk model|Applications have many environments, environments have many deployments.
