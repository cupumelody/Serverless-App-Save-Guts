# Serverless-App-Save Guts

## Stage 1:  Configure Simple Email service

<img src="https://github.com/cupumelody/Serverless-App-Save-Guts/assets/145847069/d002e371-15c5-4f93-ace9-009d224019bb" width="600" height="500">

### Stage 1A - Verify SES application sending email addresses

The Save Guts application is set to dispatch reminder messages via SMS and Email. It will leverage the Simple Email Service (SES). In a production environment, it can be configured to permit sending from the application email to any users of the application. SES initiates in sandbox mode, ensuring that messages can only be sent to verified addresses, thereby preventing spam.


There is a comprehensive process to transition SES out of sandbox mode, which you have the option to undertake. However, for the sake of this demo and to streamline the process, we will focus on verifying both the sender and receiver addresses to expedite the setup.


Ensure you are logged into an AWS account, have admin privileges and are in the `us-east-1` / N. Virginia Region.
<br>
Move to the `SES` console [https://console.aws.amazon.com/ses/home?region=us-east-1#](https://us-east-1.console.aws.amazon.com/ses/home?region=us-east-1#).
<br>
Click on `Verified Identities` under `Configuration Click Create Identity`.
<br>
Check the 'Email Address' checkbox.
<br>
Ideally you will need a `sending` email address for the application and a `receiving` email address for your test customer. But you can use the same email for both.


For my application email ... the email the app will send from I'm going to use `guts+saveguts@berserk.com`.
<br>
Enter whatever email you want to use to send in the box (it needs to be a valid address as it will be checked).
<br>
Click `Create Identity`.
<br>
You will receive an email to this address containing a link to click. Click that link.
<br>
You should see a `Congratulations!` message.
<br>
Return to the SES console and `Refresh` your browser, the verification status should now be `verified`.
<br>
Record this address somewhere save as the `Save Guts Sending Address`.

<br>

### Stage 1B - Verify SES application customer email addresses

If you want to use a different email address for the test customer (recommended), follow the steps below;
<br>
Click `Create Identity`.
<br>
Check the 'Email Address' checkbox For my application email ... the email for the test customer is `bandofthehawk+savegus@cberserk.com`.
<br>
Enter whatever email you want to use to send in the box (it needs to be a valid address as it will be checked).
<br>
Click `Create Identity`.
<br>
You will receive an email to this address containing a link to click. Click that link.
<br>
You should see a `Congratulations!` message.
<br>
Return to the SES console and refresh your browser, the verification status should now be `verified`.
<br>
Record this address somewhere save as the `Save Guts Customer Address`.

<br>

## STAGE 1 - Finish

At this point we have whitelisted 2 email addresses for use with SES.  

- the `Save Guts Sending Address`.
- the `Save Guts Customer Address`.
