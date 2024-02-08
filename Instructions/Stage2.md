# Serverless-App-Save Guts

## Stage 2:  Add an email lambda function to use SES to send emails for the serverless application

<img src="https://github.com/cupumelody/Serverless-App-Save-Guts/assets/145847069/433ca2a2-a81d-46b4-9ae1-3209cf82d0b8" width="600" height="500">

### STAGE 2A - Create the Lambda Execution Role for Lambda

In this phase of the demonstration, it's essential to establish an IAM role that the email_reminder_lambda will utilize to engage with various AWS services.

First, click this ![file](https://github.com/cupumelody/Serverless-App-Save-Guts/files/14205200/LambdaRole.zip). It contain file that will be used in this stage.
<br>
Then, open https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create in a new tab.
<br>
On the `Stack Create`:
 * on  `Prerequisite - Prepare template`, choose `Template is ready`
 * on `Specify template`, choose `Upload a template file`, and then upload the yaml file (inside that zip file), and then hit `next`.
 * on `Provide a stack name`, named it `LambdaRole`. And hit `next`
 * leave as a default, hit `next`
 * check anything, and scroll to the bottom, check on the `I acknowledge that AWS CloudFormation might create IAM resources.` box, and hit `submit`.


Wait for the Stack to move into the CREATE_COMPLETE state before moving into the next


Move to the IAM Console https://console.aws.amazon.com/iam/home?#/roles and review the execution role.
<br>
Notice that it provides SES, SNS and Logging permissions to whatever assumes this role.
<br>
This is what gives lambda the permissions to interact with those services.


### STAGE 2B - Create the email_reminder_lambda function

Create the lambda function which will will be used by the serverless application to create an email and then send it using `SES`.

Move to the lambda console https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions,
<br>
Click on `Create Function`,
<br>
Select `Author from scratch`,
<br>
For `Function name` enter `email_reminder_lambda`, and for runtime click the dropdown and pick `Python 3.9`,
<br>
Expand `Change default execution role`,
<br>
Pick to `Use an existing Role`,
<br>
Click the `Existing Role` dropdown and pick `LambdaRole` (there will be randomness and thats ok),
<br>
Click `Create Function`.


### STAGE 2C - Configure the email_reminder_lambda function

Scroll down, to `Function code`. In the `lambda_function` code box, select all the code and delete it.
<br>
Paste in this code

```
import boto3, os, json

FROM_EMAIL_ADDRESS = 'REPLACE_ME'

ses = boto3.client('ses')

def lambda_handler(event, context):
    # Print event data to logs .. 
    print("Received event: " + json.dumps(event))
    # Publish message directly to email, provided by EmailOnly or EmailPar TASK
    ses.send_email( Source=FROM_EMAIL_ADDRESS,
        Destination={ 'ToAddresses': [ event['Input']['email'] ] }, 
        Message={ 'Subject': {'Data': 'THERE COMING! Griffith coming with some lads. THERES A LOT OF THEM'},
            'Body': {'Text': {'Data': event['Input']['message']}}
        }
    )
    return 'Success!'
  
```


This function will send an email to an address it's supplied with (by step functions) and it will be FROM the email address we specify.
<br>
Select `REPLACE_ME` and replace with the `Save Guts Sending Address` which you noted down in `STAGE1`.
<br>
Click `Deploy` to configure the lambda function.
<br>
Scroll all the way to the top, and click the `copy` icon next to the lambda function ARN.
<br>
Note this ARN down somewhere same as the `email_reminder_lambda` ARN.


### STAGE 2 - FINISH   

At this point you have configured the lambda function which will be used eventually to send emails on behalf of the serverless application.






