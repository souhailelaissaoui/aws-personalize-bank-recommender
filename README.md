# Product Recommender System

This project implements a product recommender system using AWS services, including Amazon Personalize, AWS Lambda, Amazon DynamoDB, AWS Step Functions, and Amazon API Gateway. It also includes a simple web interface for interacting with the recommender system.

## Prerequisites

Before deploying this project, you need to:

1. Have an AWS account with appropriate permissions to create and manage the required services.
2. Train and deploy an Amazon Personalize campaign using the previously provided Jupyter notebooks.
3. Have the AWS CloudFormation template (`cf.yaml`) and the HTML file (`index.html`) ready.

## Deployment Steps

1. **Prepare Amazon Personalize Campaign**
   - Complete the training and deployment of your Amazon Personalize campaign using the provided Jupyter notebooks.
   - Note down the ARN (Amazon Resource Name) of your deployed campaign.

2. **Deploy CloudFormation Stack**
   - Sign in to the AWS Management Console and navigate to the CloudFormation service.
   - Click "Create stack" and choose "With new resources (standard)".
   - Under "Specify template", choose "Upload a template file" and upload your `cf.yaml` file.
   - Click "Next".
   - Enter a Stack name (e.g., "ProductRecommenderStack").
   - Under "Parameters", enter:
     - `PersonalizeCampaignArn`: The ARN of your Amazon Personalize campaign
     - `BucketName`: A unique name for the S3 bucket that will host the website
   - Click "Next", review the stack details, and create the stack.
   - Wait for the stack creation to complete. You can monitor the progress in the AWS CloudFormation console.

3. **Upload Website Files**
   - After the stack creation is complete, navigate to the S3 service in the AWS Console.
   - Find and click on the bucket you specified during stack creation.
   - Click "Upload" and select your `index.html` file.
   - Complete the upload process.

4. **Update API Endpoint**
   - In the AWS CloudFormation console, go to the Outputs tab of your stack.
   - Find the `ApiUrl` output value.
   - Open the `index.html` file locally and replace the placeholder API URL with the actual URL from the CloudFormation output.
   - Re-upload the updated `index.html` to your S3 bucket following step 3.

5. **Access the Website**
   - In the CloudFormation Outputs, find the `WebsiteURL` value.
   - Open this URL in a web browser to access your Product Recommender System.

## Using the Product Recommender System

1. Enter a User ID in the provided field.
2. Choose the type of request:
   - "Select Products" to provide specific products for consideration.
   - "Provide Metadata" to input user demographic information.
   - "Both Products and Metadata" to provide both types of information.
3. Depending on your selection, fill in the relevant fields.
4. Click "Get Recommendations" to receive personalized product recommendations.

## Architecture Overview

- **Amazon Personalize**: Provides the machine learning model for generating personalized recommendations.
- **AWS Lambda**: Processes API requests, interacts with Amazon Personalize, and writes results to DynamoDB.
- **Amazon DynamoDB**: Stores recommendation results.
- **AWS Step Functions**: Orchestrates the recommendation workflow.
- **Amazon API Gateway**: Provides the REST API for the web interface.
- **Amazon S3**: Hosts the static website files.

## Troubleshooting

- If you encounter permission issues, ensure that your AWS user has the necessary permissions to create and manage the required resources.
- For API errors, check the CloudWatch Logs for the Lambda functions to identify any issues:
  1. Navigate to the CloudWatch service in the AWS Console.
  2. Click on "Log groups" in the left sidebar.
  3. Find and click on the log group for your Lambda function (it should start with "/aws/lambda/").
  4. Review the most recent log streams for error messages.
- If the website doesn't load, verify that the S3 bucket policy is correctly set to allow public read access:
  1. Go to the S3 service in the AWS Console.
  2. Click on your bucket, then go to the "Permissions" tab.
  3. Check that "Block all public access" is turned off and that the bucket policy allows public read access.

## Cleanup

To avoid ongoing charges, remember to delete your resources when you're done:

1. Empty the S3 bucket:
   - Navigate to the S3 service in the AWS Console.
   - Select your bucket, click "Empty", and confirm the action.

2. Delete the CloudFormation stack:
   - Go to the CloudFormation service in the AWS Console.
   - Select your stack, click "Delete", and confirm the action.

3. Delete your Amazon Personalize campaign and dataset group:
   - Navigate to the Amazon Personalize service in the AWS Console.
   - Delete your campaign, solution version, solution, and dataset group in that order.

## Security Considerations

This setup uses public S3 hosting and an unauthenticated API for simplicity. For production use, consider implementing proper authentication and authorization mechanisms, and use HTTPS for all communications.
