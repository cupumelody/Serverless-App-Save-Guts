# Serverless-App-Save Guts

## Stage 6 : Clean Up

<img src="https://github.com/cupumelody/Serverless-App-Save-Guts/assets/145847069/07e73341-4d3d-4532-b22d-38fab8426d62" width="600" height="500">



Move to the S3 console https://s3.console.aws.amazon.com/s3/home?region=us-east-1
<br>
Select the bucket you created.
<br>
Click `Empty`, type or copy/paste the bucket name and click `Empty`, Click `Exit`.
<br>
Click `Delete`, type or copy/paste the bucket name and click `Delete`, Click `Exit`.

Move to the API Gateway console https://console.aws.amazon.com/apigateway/main/apis?region=us-east-1
<br>
Check the box next to the `saveguts` API.
<br>
Click `Actions` and then `Delete`
<br>
Click `Delete`.

Move to the lambda console https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions
<br>
Check the box next to `email_reminder_lambda`, click `Actions`, Click `Delete`, Click `Delete`
<br>
Check the box next to `api_lambda`, click `Actions`, Click `Delete`, Click `Delete`

Move to the Step Functions console https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines
<br>
Check the box next to `SaveGuts`, CLick `Delete`, then `Delete state machine`.

Go to the SES console and verified identities https://us-east-1.console.aws.amazon.com/ses/home?region=us-east-1#/verified-identities
<br>
Select one of the indentities, Click `Delete`, then click `Confirm`
<br>
Pick the other verified identity, Click `Delete`, then click `Confirm`.

Move to the cloudformation console https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringText=&filteringStatus=active&viewNested=true&hideStacks=false  
<br>
Check the box next to `SMROLE`, click `Delete` then `Delete Stack`.
<br>
Check the box next to `LAMBDAROLE`, click `Delete` then `Delete Stack`.

At this point we "save" Guts. But with his curse mark, it's not enough. Hope he never give up.
