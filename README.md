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
![Amplify Math Deployed Screenshot](https://github.com/jainam0037/Power-function-using-AWS/blob/main/Amplify%20Math%20Deployed.png)

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

![Lambda function ](https://github.com/user-attachments/assets/3dd8c594-fe75-4209-8470-84d03aa7529c)

```python
import json

def lambda_handler(event, context):
    base = event['base']
    exponent = event['exponent']
    result = pow(base, exponent)
    return {
        'statusCode': 200,
        'body': json.dumps({'result': result})
    }

```

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
![API Gateway(Deploying API)](https://github.com/user-attachments/assets/8392585a-c91f-412c-95ec-0ab04bd8d56f)

---

## 4. Testing the API Gateway
**AWS Services**: API Gateway + Lambda  
**Goal**: Ensure that API Gateway triggers the Lambda function correctly.

### Steps:
1. Use API Gateway’s built-in testing tool (lightning icon) to send test data (base and exponent).
2. Verify that the Lambda function was executed successfully and returned the correct result.
![deploy api](https://github.com/user-attachments/assets/bbee970c-b95a-4d12-b3b1-ce0702642e75)

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
![Testing API](https://github.com/user-attachments/assets/5315a93f-289b-4e3c-a8d5-b8246381dd48)

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




