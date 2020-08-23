# AWS Lambda

## Serverless Framework
Serverless is a framework which aims to ease the pain of creating, deploying, managing, and debugging AWS Lambda functions.

### Set up Serverless
  * Download Node
    * `brew install node`
  * Download Serverless
    * `npm install -g serverless`
  * Create `serverless-admin` IAM user on AWS
    * User name: `serverless-admin`
    * Access type: `Programmatic access`
    * Permissions: `Attach existing policies directly` -> `AdministratorAccess`
    * Tags: None
  * Configure user profile to Serverless
    * Grab `Access key ID` and `Secret` from AWS
    * Run the following command replacing arguments with $
      * `serverless config credentials --provider aws --key $aws-key --secret $aws-secret --profile $aws-user-name`
      * Example: `serverless config credentials --provider aws --key AKIAV9F2PE5HVSIQO9PN --secret S0Mw5lmf7PScwdcYDq2+diY+ShAAHXTD0cAhufOx --profile serverless-admin`

### Create Lambda Function Template
Serverless has pre-built templates that help with creating new lambda functions and to deploy them to AWS. 
To access the Serverless cli you can either use `serverless <command>` or `sls <command>`  
  * Create template with the following command:
    * `sls create --template $language --path $function-name`
    * Python template example: `sls create --template aws-python --path hello-world-python`
    * Now you'll see a new directory called `hello-world-python` with a `handler.py` and `serverless.yml` files.
    
### Deploy lambda function to AWS
Serverless can deploy your lambda function directly into AWS
* Deploy lambda function to AWS:
  * INSIDE of the lambda function directory, run: `sls deploy -v`
  * Hop over to AWS and verify that your lambda function was sucessfully deployed.

### Run lambda function locally
Serverless allows you to run your lambda function locally for testing instead of having to be on the AWS console to do it.    
* Run your lambda function locally for dev/testing:
  * INSIDE of the lambda function directory, run: `sls invoke $function-name -l`
  * The `-l` is for logging