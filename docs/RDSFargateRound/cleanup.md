# RDS and Fargate Round - Clean up

## Fargate-specific clean up

If you did the Fargate phase of this workshop round, then do the steps in this section.  Otherwise, skip to the General clean up section below.

1. If you are do not have a session open to the bastion host, then connect to the bastion host using AWS Systems Manager Session Manager.  To do this:

    1. Go to the Systems Manager console.
    2. Select **Session Manager**.
    3. Click **Start session**.
    4. Select the radio button for the instance associated with the bastion host.
    5. Click **Start session**.

2. The scripts you will be using are owned by the ec2-user account.  If you are not currently using ec2-user as your effective user id, then enter the command below to change your effective user id and directory to those of ec2-user:

    **sudo su - ec2-user**

## General clean up

!!! info  "Do not perform general clean up if you are at an *AWS event* where the *Event Engine* is being used. This is handled automatically." 

??? info  "Click here if you're *not at an AWS event* or are using your own account" 

    Now that you have seen how AWS Secrets Manager can rotate the credentials for a private database and, optionally, with AWS Fargate, please follow these steps to remove the resources you created, including the private database.

    1. [Delete the secret you created in Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_delete-restore-secret.html?shortFooter=true).  Note that when you delete a secret, the deletion is scheduled for a minimum of seven days in the future. 

    2. When you enabled rotation on your secret, AWS Secrets Manager used AWS CloudFormation to create an AWS Lambda function to do the rotation using the [AWS Serverless Application Repository](https://aws.amazon.com/serverless/serverlessrepo/).  Go to the AWS CloudFormation console and delete this stack.  The name of the stack begins with the following string:

        **aws-serverless-repository-SecretsManagerRDSMySQLRotationSingleUser**

        Look for a stack with this naming convention that was created at about the same time as you enabled rotation.  **Do not proceed to the next step until this stack has been deleted.**

    3. [Delete the main CloudFormation stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) that you created in the Build phase.  The stack deletion process may take several minutes to complete.  If the stack deletion process either pauses or fails, it may be because of an Elastic Network Interface that gets created when rotation is enabled.  In this case, [delete the Elastic Network Interface](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) and try deleting the CloudFormation stack again.