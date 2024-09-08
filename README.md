# Math Power Calculation Web Application

This project is a simple web application that allows users to input two numbers (a base and an exponent) to calculate the power of the base. It leverages various AWS services including Amplify, Lambda, API Gateway, and DynamoDB.

## AWS Services Used
1. **Amplify**: For hosting the web page.
2. **Lambda**: For backend serverless function to calculate powers.
3. **API Gateway**: To connect the frontend with the Lambda function.
4. **DynamoDB**: To store the calculation results.
5. **IAM**: To manage permissions for Lambda to access DynamoDB.

---

## 1. Setting Up the AWS Amplify Web Application
**AWS Service**: Amplify  
**Goal**: To host a simple web page where users can input numbers and trigger the power calculation.

### Steps:
1. **Upload the HTML page**: Create an `index.html` page, which provides the UI for inputting numbers and a button to trigger the calculation.
2. **Hosting using Amplify**: Deploy the web page by uploading a zip file containing the HTML to Amplify for hosting.

---

## 2. Creating the AWS Lambda Function
**AWS Service**: Lambda  
**Goal**: To calculate the power of the base using the exponent.

### Steps:
1.	Create a Lambda Function: You created a Lambda function named power of math that accepts the base and exponent and returns the result.
2.	Code the Lambda Function: Wrote Python code that reads input, calculates the power using Python’s pow() function, and returns the result.
3.	Navigate to the AWS Lambda console.
4.	Click Create Function and select the "Author from scratch" option.
5.	Name your function, e.g., power_of_math.
6.	Choose Python as the runtime language and set up the execution role with basic Lambda permissions.
7.	Click Create to create the Lambda function.
8.	Replace the placeholder Lambda function code with the actual code that calculates the power of a number:
9.	Save the code and Deploy it so the Lambda function can be triggered by API Gateway later.
10.	Test the function by providing a sample input, e.g., base = 2 and exponent = 4.
11.	Use the Lambda test feature to simulate sending this data. The result should return 16.


```python
# import the JSON utility package
import json
# import the Python math library
import math

# import the AWS SDK (for Python the package name is boto3)
import boto3
# import two packages to help us with dates and date formatting
from time import gmtime, strftime

# create a DynamoDB object using the AWS SDK
dynamodb = boto3.resource('dynamodb')
# use the DynamoDB object to select our table
table = dynamodb.Table('PowerOfMathDatabase')
# store the current time in a human readable format in a variable
now = strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())

# define the handler function that the Lambda service will use an entry point
def lambda_handler(event, context):

# extract the two numbers from the Lambda service's event object
    mathResult = math.pow(int(event['base']), int(event['exponent']))

# write result and time to the DynamoDB table using the object we instantiated and save response in a variable
    response = table.put_item(
        Item={
            'ID': str(mathResult),
            'LatestGreetingTime':now
            })

# return a properly formatted JSON object
    return {
    'statusCode': 200,
    'body': json.dumps('Your result is ' + str(mathResult))
    }
```

## Save and Deploy the Lambda Function
After writing the code for the Lambda function, save it and deploy it for the changes to take effect.

### Steps:
1. **Save the Lambda Code**: After modifying the Lambda function code, click **Save**.
2. **Deploy the Function**: Click **Deploy** to ensure that the updated function is live and ready to be triggered.
3. **Test the Function**: Test the Lambda function by providing sample input. For example:
   - Base = 2, Exponent = 4
   - Expected Result: 16

---

## 3. Creating API Gateway Integration
**AWS Service**: API Gateway  
**Goal**: To allow external applications to interact with the Lambda function via HTTP requests.

### Steps:
1. Navigate to **API Gateway** and create a new **REST API**.
2. Create a **POST** method to accept the base and exponent from the user.
3. Under **Integration Type**, select **Lambda Function** and choose the `power_of_math` Lambda function.
4. Enable **CORS** to allow the front-end to make requests to the API.
5. Deploy the API and create a new stage (e.g., `dev`).
6. Copy the **Invoke URL** of the API for use in the front-end application.

---

## 4. Testing the API Gateway
**AWS Services**: API Gateway + Lambda  
**Goal**: Ensure that API Gateway triggers the Lambda function correctly.

### Steps:
1. Use API Gateway’s built-in testing tool (lightning icon) to send test data (base and exponent).
2. Verify that the Lambda function was executed successfully and returned the correct result.

---

## 5. Setting Up DynamoDB Table
**AWS Service**: DynamoDB  
**Goal**: To store the results of calculations for future reference.

### Steps:
1. Navigate to DynamoDB and create a new table named `power_of_math_database`.
2. Set `id` as the partition key.
3. Save the **ARN** (Amazon Resource Name) of the table for future use in the Lambda function’s permissions.

---

## 6. Updating Lambda Permissions for DynamoDB
**AWS Services**: Lambda + IAM  
**Goal**: To allow the Lambda function to write results to the DynamoDB table.

### Steps:
1. Go to the Lambda function’s **Configuration** tab.
2. Click on the **Execution Role** and add an **inline policy**.
3. Allow the `dynamodb:PutItem` action on the specific DynamoDB table ARN.

---

## 7. Connecting Amplify with API Gateway
**Goal**: To make the web page trigger the API Gateway.

### Steps:
1. Update the `index.html` page to make a **POST** request to the API Gateway endpoint using the user input (base and exponent).
2. Add JavaScript to the HTML page to call the API and pass the input values for processing.

---

## 8. Testing the Complete Workflow
**Goal**: To verify that the entire system functions as expected.

### Steps:
1. Deploy the updated HTML page by creating a new zip file and redeploying it using Amplify.
2. Test the web app by entering values (e.g., base = 2, exponent = 8) and clicking **Calculate**. Verify the correct result (e.g., 256) is displayed.

---

## 9. Clean-Up
**Goal**: To delete AWS resources to avoid charges.

### Steps:
1. Delete the **Amplify app** from the AWS console.
2. Delete the **power_of_math_database** table from DynamoDB.
3. Remove the **Lambda function**.
4. Delete the **API Gateway configuration**.

---

## Conclusion
This web application demonstrates how to create a serverless architecture using AWS services to calculate powers of numbers, store the results in DynamoDB, and connect the front-end to the back-end using API Gateway. The system leverages AWS Amplify for hosting, Lambda for serverless backend computation, API Gateway for API management, and DynamoDB for data persistence.
