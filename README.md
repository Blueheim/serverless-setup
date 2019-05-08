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

'use strict';

// Lambda needs to run a function, here all the logic need to be in the hello function
module.exports.hello = async (event) => {
  // Whatever trigger the function is in the event object
  // that can be an http request, an s3 request...
  // Lambda functions can use external resources like an s3 bucket
  // As a stateless entity, lambda don't remember anything as itself: it is stored in a database
  // and when it's ready to be invoked, it's gonna be put into a container by amazon and
  // gonna get run until it is not called anymore and get put back in the database.
  // The container being down it doesn't keep anything.
  return {
    statusCode: 200,
    body: JSON.stringify({
      message: 'Go Serverless v1.0! Your function executed successfully!',
      input: event,
    }, null, 2),
  };

  // Use this code if you don't use the http event with the LAMBDA-PROXY integration
  // return { message: 'Go Serverless v1.0! Your function executed successfully!', event };
};


```

## serverless.yml file
*some defaults can be overwritten*

```yml

service: my-service

# change runtime to the latest version
runtime: nodejs8.10

# change region
stage: dev or prod
region: eu-west-3

functions:
  # function name
  hello:
    # reference to the handler.js function
    handler: handler.hello
    # configure the events to be used with our function, here our function is to be called via GET request .../hello
    # used by another service by aws called api gateway to give us an http endpoint to actually trigger this event
    events:
      - http:
          path: hello
          method: get

```

## connect serverless framework
*Make sure to have a IAM user with programmatic access*

```
sls config credentials --provider aws --key AWS_USER_ID_KEY --secret AWS_USER_SECRET_KEY

> Serverless: Setting up AWS...
> Serverless: Saving your AWS profile in "~/.aws/credentials"...
> Serverless: Success! Your AWS access keys were stored under the "default" profile.

```

## Deploy the function

```
sls deploy


> Serverless: Packaging service...
> Serverless: Excluding development dependencies...
> Serverless: Creating Stack...
> Serverless: Checking Stack create progress...
> .....
> Serverless: Stack create finished...
> Serverless: Uploading CloudFormation file to S3...
> Serverless: Uploading artifacts...
> Serverless: Uploading service my-service.zip file to S3 (386 B)...
> Serverless: Validating template...
> Serverless: Updating Stack...
> Serverless: Checking Stack update progress...
> ................
> Serverless: Stack update finished...
> Service Information
> service: my-service
> stage: dev
> region: eu-west-3
> stack: my-service-dev
> resources: 10
> api keys:
>   None
> endpoints:
>   GET - https://[id].execute-api.eu-west-3.amazonaws.com/dev/hello
> functions:
>   hello: my-service-dev-hello
> layers:
>   None

```

## Call the newly deployed function

```
sls invoke --function hello

> {
>     "statusCode": 200,
>     "body": "{\n  \"message\": \"Go Serverless v1.0! Your function executed successfully!\",\n  \"input\": {}\n}"
> }

```

## Call the function locally for avoiding aws charging
*Only work for simple functions which do not depend on other aws resources like s3*

```
sls invoke local --function hello

> {
>     "statusCode": 200,
>     "body": "{\n  \"message\": \"Go Serverless v1.0! Your function executed successfully!\",\n  \"input\": {}\n}"
> }

```

## Use the function in an app
*We use the endpoint given when deploying*

GET https://[id].execute-api.eu-west-3.amazonaws.com/dev/hello


```json

{
  "message": "Go Serverless v1.0! Your function executed successfully!",
  "input": {
  "resource": "/hello",
  "path": "/hello",
  "httpMethod": "GET",
  ...
}

```
