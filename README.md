## Essential serverless setup for AWS Lambda functions

## Serverless install


```js
//https://www.npmjs.com/package/serverless
//Global installation makes sense for this package

npm i serverless -g 
```

#Help

```
serverless (or sls)
sls create --help

```

#Creation of a new serverless service using aws-nodejs template

```
sls create -t aws-nodejs --path my-service
cd my-service

```

## handler.js function file

```js


```

## serverless.yml file
*some defaults can be overwritten*

```yml

# change runtime to the latest version
runtime: nodejs8.10

# change region
region: eu-west-3

functions:
  # function name
  hello:
    # reference to the handler.js function
    handler: handler.hello

```

## connect serverless framework
*Make sure to have a IAM user with programmatic access*

```
sls config credentials --provider aws --key AWS_USER_ID_KEY --secret AWS_USER_SECRET_KEY

> Serverless: Setting up AWS...
> Serverless: Saving your AWS profile in "~/.aws/credentials"...
> Serverless: Success! Your AWS access keys were stored under the "default" profile.

```

