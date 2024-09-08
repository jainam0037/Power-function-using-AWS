# Math Application using AWS

This project demonstrates how to create a simple web application that calculates the power of a base number raised to an exponent, using various AWS services like Amplify, Lambda, API Gateway, and DynamoDB.

## 1. Setting Up the AWS Amplify Web Application
- **AWS Service**: Amplify
- **Goal**: To host a simple web page where users can input two numbers (a base and an exponent) to calculate the power of the base.

### Steps:
1. **Upload the HTML Page**: Created an `index.html` page which provides the UI for inputting numbers and a button to trigger the calculation.
2. **Hosting using Amplify**: Deployed the web page by uploading a zip file containing the `index.html` to Amplify for hosting.

---

## 2. Creating the AWS Lambda Function
- **AWS Service**: Lambda
- **Goal**: To calculate the power of the base using the exponent.

### Steps:
1. **Create a Lambda Function**: Created a Lambda function named `power_of_math` that accepts the base and exponent as input and returns the result.
2. **Code the Lambda Function**: Wrote Python code to read the input, calculate the power using Python’s `pow()` function, and return the result.
3. **Lambda Function Setup**:
   - Navigate to the AWS Lambda console.
   - Click **Create Function** and select "Author from scratch".
   - Name the function `power_of_math` and choose Python as the runtime.
   - Set up the execution role with basic Lambda permissions.
4. **Code Example**:
   ```python
   def lambda_handler(event, context):
       base = event['base']
       exponent = event['exponent']
       result = base ** exponent
       return {"result": result}
