# Serverless-App-Save Guts

## Stage 4:  API Gateway and Application Lambda

<img src="https://github.com/cupumelody/Serverless-App-Save-Guts/assets/145847069/1c992a83-6ef3-47c5-b6d1-0e604f4d4446"  width="600" height="500">

### STAGE 4A - Create API Lambda function which support API Gateway

Move to the Lambda console https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions ,
<br>
Click on `Create Function`;
* for `Function Name` use `api_lambda`,
* for `Runtime` use `Python 3.9`,
* Expand `Change default execution role`,
* Select `Use an existing role`,
* Choose the `LambdaRole` from the dropdown,
* Click `Create Function`  

<br>

### STAGE 4B - Configure the Lambda Function (Using the current UI)

Scroll down, and remove all the code from the `lambda_function` text box.
<br>
Open this [link](https://github.com/cupumelody/Serverless-App-Save-Guts/files/14210587/api_lambda.zip) ,
<br>
Depending on your browser it might download the .py file, if so, open it in either your code editor or notepad, and copy it all into your clipboard.
<br>
Move back to the Lambda console.
<br>
Select the existing lambda code and delete it.
<br>
Paste the code into the lambda fuction.  

This is the function that will provide compute to API Gateway.
<br>
Its job is to be called by API Gateway when it's used by the serverless front end part of the application (loaded by S3).
It accepts some information from you, via API Gateway and then it starts a state machine execution - which is the logic of the application.  

You need to locate the `YOUR_STATEMACHINE_ARN` placeholder and replace this with the State Machine ARN you noted down in the previous step.

<br>

### STAGE 4C - Create API

The next step is to create the API Gateway, API and Method which the front end part of the serverless application will communicate with.  


Move to the API Gateway console https://console.aws.amazon.com/apigateway/main/apis?region=us-east-1 .
<br>
Click `APIs` on the menu on the left,
<br>
Locate the `REST API` box, and click `Build` (being careful not to click the build button for any of the other types of API ... REST API is the one you need),
<br>
If you see a popup dialog `Create your first API` dismiss it by clicking `OK`,
<br>
Under `Create new API` ensure `New API` is selected.
* For `API name*` enter `saveguts`
* for `Endpoint Type` pick `Regional`
* Click `create API`
* Click `Deploy` to save the lambda function and configuration.

<br>

### STAGE 4D - Create Resource

Click the `Actions` dropdown and Click `Create Resource`.
<br>
Under resource name enter `petcuddleotron`.
<br>
Make sure that `Configure as proxy resource` is **NOT** ticked - this forwards everything as is, through to a lambda function, because we want some control, we **DONT** want this ticked.
<br>
Towards the bottom **MAKE SURE TO TICK** `Enable API Gateway CORS`.
<br>
This relaxes the restrictions on things calling on our API with a different DNS name, it allows the code loaded from the S3 bucket to call the API gateway endpoint. 
<br>
**if you DONT check this box, the API will fail**
<br>
Click `Create Resource`.

<br>

### STAGE 4E - Create Method

Ensure you have the `/saveguts` resource selected, click `Actions` dropdown and click `create method`.
<br>
In the small dropdown box which appears below `/saveguts` select `POST` and click the `tick` symbol next to it. This method is what the front end part of the application will make calls to.  
Its what the api_lambda will provide services for.
<br>
Ensure for `Integration Type` that `Lambda Function` is selected.
<br>
Make sure `us-east-1` is selected for `Lambda Region`
<br>
In the `Lambda Function` box.. start typing `api_lambda` and it should autocomplete, click this auto complete (**Make sure you pick api_lambda and not email reminder lambda**)
* Make sure that `Use Default Timeout` box **IS** ticked.
* Make sure that `Use Lambda Proxy integration` box **IS** ticked, this makes sure that all of the information provided to this API is sent on to lambda for processing in the `event` data structure.  
* **if you don't tick this box, the API will fail**
Click `Save`.

<br>

### STAGE 4F - Deploy API

Now the API, Resource and Method are configured - you need to deploy the API out to API gateway, specifically an API Gateway STAGE.  


Click `Actions` Dropdown and `Deploy API`.
<br>
For `Deployment Stage` select `New Stage`.
* for stage name and stage description enter `prod`
Click `Deploy`.

At the top of the screen will be an `Invoke URL` .. note this down somewhere safe, you will need it in the next STAGE.
<br>
This URL will be used by the client side component of the serverless application and this will be unique to you.
<br>
You may see a dialogue stating `You are about to give API Gateway permission to invoke your Lambda function:`. AWS is asking for your OK to adjust the `resource policy` on the lambda function to allow API Gateway to invoke it.  This is a different policy to the `execution role policy` which controls the permissions lambda gets.

<br>

### STAGE 4 - FINISH

At this point you have configured the last part of the AWS side of the serveless application.   
You now have :-

- SES Configured
- An Email Lambda function to send email using SES
- A State Machine configured which can send EMAIL after a certain time period when invoked.
- An API, Resource & Method, which use a lambda function for backing deployed out to the PROD stage of API Gateway
