# AWS Lambda

## Serverless Framework
Serverless is a framework which aims to ease the pain of creating, deploying, managing, and debugging AWS Lambda functions.

### Setting up Serverless
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
  