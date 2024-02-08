# Serverless-App-Save Guts

## Stage 3 : Implement and configure the state machine, the core of the application

<img src="https://github.com/cupumelody/Serverless-App-Save-Guts/assets/145847069/9ee6025a-3bc6-414f-828d-3a318b2bbdfd" width="600" height="500">

# STAGE 3A - Create State Machine Role

In this stage of the demo you need to create an IAM role which the state machine will use to interact with other AWS services.

First, click this ![file](StateMachineRole.zip(https://github.com/cupumelody/Serverless-App-Save-Guts/files/14205796/StateMachineRole.zip).It contain file that will be used in this stage.
<br>
Then, open https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create in a new tab.
<br>
On the `Stack Create`:
 * on  `Prerequisite - Prepare template`, choose `Template is ready`
 * on `Specify template`, choose `Upload a template file`, and then upload the yaml file (inside that zip file), and then hit `next`.
 * on `Provide a stack name`, name it `StateMachineRole`. And hit `next`
 * leave as a default, hit `next`
 * check anything, and scroll to the bottom, check on the `I acknowledge that AWS CloudFormation might create IAM resources.` box, and hit `submit`.

Wait for the Stack to move into the CREATE_COMPLETE state before moving into the next

Move to the IAM Console https://console.aws.amazon.com/iam/home?#/roles and review the STATE MACHINE role
note how it gives 

- logging permissions
- the ability to invoke the email lambda function when it needs to send emails
- the ability to use SNS to send text messages


### STAGE 3B - Create State Machine
Move to the AWS Step Functions Console https://console.aws.amazon.com/states/home?region=us-east-1#/homepage ,
<br>
Click the `Hamburger Menu` at the top left and click `State Machines`,
<br>
Click `Create State Machine`,
<br>
Select `Write your workflow in code` which will allow you to use Amazon States Language,
<br>
Scroll down,
for `type` select `standard`,
<br>
Open this in your notepad [SMJson.txt](https://github.com/cupumelody/Serverless-App-Save-Guts/files/14206085/SMJson.txt),
<br>
This is the Amazon States Language (ASL) file for the Save Guts state machine.
* Copy the contents into your clipboard,
* Move back to the step functions console,
* Select all of the code snippet and delete it,
* Paste in your clipboard.

Click the `Refresh` icon on the right side area, next to the visual map of the state machine.
<br>
Look through the visual overview and the ASL and make sure you understand the flow through the state machine.
<br>
The state machine starts and then waits for a certain time period based on the `Timer` state. This is controlled by the web front end you will deploy soon. Then the `email` is used Which sends an email reminder
<br>
The state machine will control the flow through the serverless application.. once stated it will coordinate other AWS services as required.  

### STAGE 3C - Configure State Machine 
In the state machine ASL (the code on the left) locate the `EmailOnly` definition.  
<br>
Look for `EMAIL_LAMBDA_ARN` which is a placeholder, replace this with the email_reminder_lambda ARN you noted down in the previous step. This is the ARN of the lambda function you created.

Scroll down to the bottom and click `next`,
<br>
For `State machine name` use `SaveGuts`,
<br>
Scroll down and under `Permissions` select `Choose an existing role` and select `StateMachineRole` from the dropdown,
<br>
Scroll down, under `Logging`, change the `Log Level` to `All`,
<br>
Scroll down to the bottom and click `Create state machine`.


Locate the `ARN` for the state machine on the top left... note this down somewhere safe as `State Machine ARN`  


### STAGE 3 - FINISH
At this point you have configured the state machine which is the core part of the serverless application.  
The state machine controls the flow through the application and is responsible for interacting with other AWS products and services. 
