# Flow-Logs-to-CEF-Conversion
Pulls VPC Flow Logs from Cloudwatch logs, converts to CEF format, and deposits in an S3 bucket.

# IAM_User
Creates an IAM Group called ArcSightGroup an IAM user called ArcSightUser with access keys and assigns the following permissions:
S3:GetObject
S3:ListBucket

# IAM_Lambda_Role
This creates an IAM Role for the Lambda function to leverage and grants the following permissions:
S3 Access
Logs Access
Lambda Access
Cloudformation Access
CloudWatch Access

This also exports the role naming it LambdaRoleForArcSight which gets imported by the Lambda creation Cloudformation

# CF_For_Lambda_Function
This Cloudformation launches a Lambda function, creates an S3 bucket, subscribes the Lambda function to the VPC Flow Log stream in CloudWatch, and assigns the applicable permissions for all to work, importing the role from the IAM_Lambda_Role. 

Lambda Function - This function is triggered by the CloudWatch Logs VPC Flow Logs log group that was input at stack launch. When a log is delivered, it is converted from VPC Flow Log Format to Commen Event Format (CEF) dictionary format. It places these converted logs in the S3 bucket created at stack launch. 

S3 Bucket - Receives the converted logs from the Lambda function

