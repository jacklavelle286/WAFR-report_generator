# Well-Architected Report Generation Tool

The Well-Architected Report Generation Tool is a serverless solution designed to automate the process of generating, storing, and distributing reports based on the AWS Well-Architected Framework's reviews. This tool leverages AWS services such as Lambda, DynamoDB, S3, SNS, Step Functions, and EventBridge to create an efficient and scalable workflow.

## Architectural Overview

![Slide1](https://github.com/jacklavelle286/WAFR-report_generator/assets/78485499/700cc44a-aa9d-47cb-acb3-f174cf8a5a5a)

## Overview

This tool performs the following tasks:

1. Triggers the workflow when a new milestone is created in the AWS Well-Architected Tool.
2. Stores relevant data from the Well-Architected Review in a DynamoDB table.
3. Generates a CSV report containing high and medium risk items.
4. Creates a comprehensive Word document report using a predefined template, embedding the CSV data and additional analysis.
5. Uploads the report to an S3 bucket and sends a notification with a presigned URL to a specified email address, allowing secure and easy access to the report.

## Deployment Steps

### Prerequisites

- AWS Account and CLI access with appropriate permissions.
- Knowledge of AWS services such as Lambda, DynamoDB, S3, SNS, Step Functions, and EventBridge.

### Steps

1. **Prepare the Template and Email Configuration**
   
   - Update the `TemplateFile` parameter with the name of your report template stored in an S3 bucket.
   - Set the `ReportEmail` parameter with the desired recipient's email address for report notifications.

2. **Deploy the CloudFormation Stack**

   - Use the AWS CloudFormation console or AWS CLI to deploy the provided template. This will set up all the necessary resources.

3. **Upload Your Template**

   - Ensure your Word document template is uploaded to the `war-template-bucket` created by the CloudFormation stack.

4. **Triggering the Workflow**

   - The workflow is automatically triggered when a new milestone is created in the AWS Well-Architected Tool. Ensure that CloudTrail is enabled and capturing events for the Well-Architected Tool.

5. **Accessing the Report**

   - Once the workflow is complete, the designated recipient will receive an email with a presigned URL to download the report from the S3 bucket.

## Architecture

The tool's architecture consists of several AWS services working together:

- **EventBridge**: Captures the creation of new milestones from the Well-Architected Tool.
- **Step Functions**: Orchestrates the workflow of the report generation process.
- **Lambda Functions**: Execute the logic for each step of the workflow, including interacting with the Well-Architected Tool, DynamoDB, S3, and SNS.
- **DynamoDB**: Stores extracted data from the Well-Architected Review for processing.
- **S3 Buckets**: Hosts the report template, stores the generated CSV and Word document reports.
- **SNS**: Sends notifications with presigned URLs to access the generated reports.

