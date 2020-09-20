# AWS Lambda

## Serverless Framework
Serverless is a framework which aims to ease the pain of creating, deploying, managing, and debugging AWS Lambda functions.

### Set up Serverless
<details>
  <summary>How to set up serverless locally.</summary>

<br>

  * Download Node:
    * `brew install node`
  * Download Serverless:
    * `npm install -g serverless`
  * Create `serverless-admin` IAM user on AWS:
    * User name: `serverless-admin`
    * Access type: `Programmatic access`
    * Permissions: `Attach existing policies directly` -> `AdministratorAccess`
    * Tags: None
  * Configure user profile to Serverless:
    * Grab `Access key ID` and `Secret` from AWS.
    * Run the following command replacing arguments with $:
      * `serverless config credentials --provider aws --key $aws-key --secret $aws-secret --profile $aws-user-name`
      * Example: `serverless config credentials --provider aws --key AKIAV9F2PE5HVSIQO9PN --secret S0Mw5lmf7PScwdcYDq2+diY+ShAAHXTD0cAhufOx --profile serverless-admin`
</details>

### Create Lambda Function Template
<details>
  <summary>How to create lambda functions with serverless templates.</summary>
 
<br>
 
Serverless has pre-built templates that help with creating new lambda functions and to deploy them to AWS. 
To access the Serverless cli you can either use `serverless <command>` or `sls <command>`.
  * Create template with the following command:
    * `sls create --template $language --path $function-name`.
    * Python template example: `sls create --template aws-python --path hello-world-python`.
    * Now you'll see a new directory called `hello-world-python` with a `handler.py` and `serverless.yml` files.
  
> Run `sls create --help` for all argument options.  
  
### Update `serverless.yml`
In this file you will see that it is pre-filled with a lot of things, and most of it is commented out. Travel to the
`provider` section and let's make the following changes:
  * add `profile: <your-iam-user>`
  * add `region <your-aws-region>`  
  
 Example of changes:
```
provider:
  name: aws
  runtime: python2.7
  profile: serverless-admin
  region: us-east-2
```
</details>
 
### Deploy lambda function to AWS
<details>
  <summary>How to deploy to AWS with serverless.</summary>
  
<br>

> Keep in mind that this deploys the WHOLE stack to AWS, not just your one function that you may have updated.
> When you initially create a new serverless app, you will HAVE TO deploy the whole stack, but afterwards you are free
> to just deploy the needed functions.
> This deployment of the stack depending on the size of your application can end up taking a long time. Read the
> `Update and deploy lambda function` section to learn how to only deploy specific functions instead of the whole stack.      
  
Serverless can deploy your lambda function directly into AWS.
* Deploy lambda function to AWS:
  * INSIDE of the lambda function directory, run: `sls deploy -v`.
  * Hop over to AWS and verify that your lambda function was successfully deployed.
  
> Run `sls deploy --help` for all argument options.
  
#### Errors encountered:
<details>
  <summary>Serverless deploy - Function not found</summary>

```
(base) ~/PycharmProjects/aws-lambda/hello-world-python$ sls deploy -v
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Uploading service hello-world-python.zip file to S3 (220 B)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
CloudFormation - UPDATE_IN_PROGRESS - AWS::CloudFormation::Stack - hello-world-python-dev
CloudFormation - UPDATE_IN_PROGRESS - AWS::Lambda::Function - HelloLambdaFunction
CloudFormation - UPDATE_FAILED - AWS::Lambda::Function - HelloLambdaFunction
 
Serverless Error ---------------------------------------
An error occurred: HelloLambdaFunction - Function not found: arn:aws:lambda:us-east-2:410568828559:function:hello-world-python-dev-hello (Service: AWSLambdaInternal; Status Code: 404; Error Code: ResourceNotFoundException; Request ID: 1295f3c6-4621-4cc6-a0ce-8859f3a56c90; Proxy: null).
```

Resolution:
* Open up your `serverless.yml` file and comment out the function having the issue under the `functions` section in this file.
* Run a `sls deploy`, it should successfully deploy this time.
* Uncomment your function in the `serverless.yml`
* Run a `sls deploy`, your function should successfully deploy this time.
* Source: [stackoverflow](https://stackoverflow.com/questions/58382779/serverless-deploy-function-not-found)
</details>
</details>

### Update and deploy lambda function 
<details>
  <summary>How to deploy a specific lambda function only.</summary>

<br>

* Make all necessary changes to your function. 
* Run `sls deploy function -f <function name>`
* Example: 
```
(base) ~/PycharmProjects/aws-lambda/hello-world-python$ sls deploy function -f hello
Serverless: Packaging function: hello...
Serverless: Excluding development dependencies...
Serverless: Uploading function: hello (207 B)...
Serverless: Successfully deployed function: hello
Serverless: Successfully updated function: hello
(base) ~/PycharmProjects/aws-lambda/hello-world-py
```

> Run `sls deploy --help` for all argument options.
</details>

### Run lambda function locally
<details>
  <summary>How to run lambda functions locally with serverless.</summary>
  
<br>

Serverless allows you to run your lambda function locally for testing instead of having to be on the AWS console to do it.    
* Run your lambda function locally for dev/testing:
  * INSIDE of the lambda function directory, run: `sls invoke $function-name -l`.
  * The `-l` is for logging.
  
> Run `sls invoke --help` for all argument options.  
</details>

### Fetching function logs
<details>
  <summary>How to fetch function logs.</summary>

<br>

Once your function is up and live, you will not always be the only one invoking it but you will need to be able to
retrieve logs for specific executions. AWS stores all function execution logs, serverless allows you to fetch and view
these logs via their CLI as well.

> These logs can also be view on the AWS console.
    
* Run `sls logs -f <function name> -t`
  * `-f` option is a required argument for the function name.
  * `-t` option is optional to tail the log output.
  
> Run `sls logs --help` for all argument options.
</details>


### Destroying the service
<details>
  <summary>How to complete remove the service off AWS.</summary>

<br>

Serverless allows you to fully remove/delete the service you have built. By doing so, we run into a few problems.

We will need to clean up the following:
1. Function
2. Dependencies on the function
3. CloudWatch Log groups
4. IAM Roles
5. Everything else the framework has created

> These logs can also be view on the AWS console.
    
* Run `sls logs -f <function name> -t`
  * `-f` option is a required argument for the function name.
  * `-t` option is optional to tail the log output.
  
> Run `sls logs --help` for all argument options.
</details>