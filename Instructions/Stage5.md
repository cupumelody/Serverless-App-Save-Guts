# Serverless-App-Save Guts

## STAGE 5 : Implement the static frontend application and test functionality

<img src="https://github.com/cupumelody/Serverless-App-Save-Guts/assets/145847069/36da7e69-ad03-4c7c-bf4d-7d1ef5ea83b0" width="600" height="500">

# STAGE 5A - Create the s3 bucket

Move to the S3 Console https://s3.console.aws.amazon.com/s3/home?region=us-east-1#
<br>
Click `Create bucket`.
<br>
Choose a unique bucket name.
<br>
Ensure the region is set to `US East (N.Virginia) us-east-1`.
<br>
Scroll Down and **UNTICK** `Block all public access`
<br>
Tick the box under `Turning off block all public access might result in this bucket and the objects within becoming public` to acknowledge you understand that you can make the bucket public.  
<br>
Scroll Down to the bottom and click `Create bucket`.

# STAGE 5B - SET THE BUCKET AS PUBLIC

Go into the bucket you just created.  
Click the `Permissions` tab.  
Scroll down and in the `Bucket Policy` area, click `Edit`. 


in the box, paste the code below

```
{
    "Version":"2012-10-17",
    "Statement":[
      {
        "Sid":"PublicRead",
        "Effect":"Allow",
        "Principal": "*",
        "Action":["s3:GetObject"],
        "Resource":["REPLACEME_SAVE_GUTS_BUCKET_ARN/*"]
      }
    ]
  }

```
Replace the `REPLACEME_PET_CUDDLE_O_TRON_BUCKET_ARN` (being careful NOT to include the `/*`) with the bucket ARN, which you can see near to `Bucket ARN `
Click `Save Changes`  

<br>

### STAGE 5C - Enable Static Hosting
Next we need to enable static hosting on the S3 bucket so that it can be used as a front end website.


Click on the `Properties Tab`.
<br>
Scroll down and locate `Static website hosting`
<br>
Click `Edit`.
<br>
Select `Enable`.
<br>
Select `Host a static website`
<br>
For both `Index Document` and `Error Document` enter `index.html`
<br>
Click `Save Changes`.
<br>
Scroll down and locate `Static website hosting` again.
<br>
Under `Bucket Website Endpoint` copy and note down the bucket endpoint URL.  

<br>

### STAGE 5D - Download and edit front end files

Download and extra this ZIP [file](https://github.com/cupumelody/Serverless-App-Save-Guts/files/14211462/serverless-frontend.zip)

Inside the serverless_frontend folder are the front end files for the serverless website :-

- index.html .. the main index page
- main.css .. the stylesheet for the page
- skullknight.png .. an image of our saviour !!
- serverless.js .. the JS code which runs in your browser. It responds when buttons are clicked, and passes and text from the boxes when it calls the API Gateway endpoint.  

Open the `serverless.js` in a code/text editor.
<br>
Locate the placeholder `REPLACEME_API_GATEWAY_INVOKE_URL` . replace it with your API Gateway Invoke URL.
<br>
at the end of this URL.. add `/saveguts`
<br>
it should look something like this `https://somethingsomething.execute-api.us-east-1.amazonaws.com/prod/saveguts`
<br>
Save the file.  

<br>

### STAGE 5E - Upload and test

Return to the S3 console.
<br>
Click on the `Objects` Tab.
<br>
Click `Upload`.
<br>
Drag the 4 files from the serverless_frontend folder onto this tab, including the serverless.js file you just edited.
<br>
**MAKE SURE ITS THE EDITED VERSION**


Click `Upload` and wait for it to complete.
<br>
Click `Exit`.
<br>
Verify All 4 files are in the `Objects` area of the bucket.  


Open the `SaveGuts URL` you just noted down in a new tab.
<br>
What you are seeing is a simple HTML web page created by the HTML file itself and the `main.css` stylesheet.
<br>
When you click buttons .. that calls the `.js` file which is the starting point for the serverless application.


To test the application:
<br>
Enter an amount of time until the next cuddle ...I suggest `120` seconds.
<br>
Enter a message, i suggest `SAVE GUTS NOW`, then enter the `SaveGuts Customer Address` in the email box, this is the email which you verified right at the start as the customer for this application.  

**before you do the next step and click the button on the application, if you want to see how the application works do the following**
<br>
Open a new tab to the `Step functions console` https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines.
<br>
Click on `SaveGuts`
<br>
Click on the `Logging` tab, you will see no logs
<br>
CLick on the `Executions` tab, you will see no executions.

Move back to the web application tab (s3 bucket), then click on `Email Minion` Button to send an email.

Got back to the Step functions console, make sure the `Executions` Tab is selected.
<br>
click the `Refresh` Icon. Click the `execution`
<br>
Watch the graphic .. see how the `Timer state` is highlighted.
<br>
The step function is now executing and it has its own state ... its a serverless flow.
<br>
Keep waiting, and after 120 seconds the visual will update showing the flow through the state machine

- `Timer` .. waits 120 seconds
- `Email` invokes the lambda function to send an email
- `NextState` in then moved through, then finally `END`

Scroll to the top, click `ExeuctionInput` and you can see the information entered on the webpage.
<br>
This was send it, via the `JS` running in browser, to the API gateway, to the `api_lambda` then through to the `statemachine`

Click `PetCuddleOTron` at the top of the page.
<br<
Click on the `Logging` Tab.
<br>
Because the roles you created had `CWLogs` permissions the state machine is able to log to CWLogs
<br>
Review the logs and ensure you are happy with the flow.

<br>

### STAGE 5 - FINISH

Now we have a fully functional serverless application.

- Loads HTML & JS From S3 & Static hosting
- Communicates via `JS` to API Gateway 
- uses `api_lambda` as backing resource
- runs a statemachine passing in parameters
- state machine sends email
- state machine terminates
- AND MOST IMPORTANT,TO ASK SKULL KNIGHT TO SAVE GUTS!!!!
