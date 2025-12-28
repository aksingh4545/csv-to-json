<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>AWS ETL Pipeline - CSV to JSON</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 40px;
            background-color: #f9f9f9;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        code {
            background: #eef;
            padding: 4px 6px;
            display: inline-block;
        }
        pre {
            background: #272822;
            color: #f8f8f2;
            padding: 15px;
            overflow-x: auto;
        }
        img {
            max-width: 100%;
            border: 1px solid #ccc;
            margin: 10px 0;
        }
        ul {
            margin-left: 20px;
        }
    </style>
</head>
<body>

<h1>AWS ETL Pipeline: CSV to JSON</h1>

<p>
This project demonstrates an <b>AWS ETL pipeline</b> where a CSV file uploaded to Amazon S3
automatically triggers an AWS Lambda function, which starts an AWS Glue ETL job to convert
CSV data into JSON format and store the output back in Amazon S3.
</p>

<hr>

<h2>Architecture Overview</h2>
<ul>
    <li>Source: Amazon S3 (CSV file upload)</li>
    <li>Trigger: AWS Lambda (S3 PUT event)</li>
    <li>ETL Processing: AWS Glue</li>
    <li>Target: Amazon S3 (JSON output)</li>
</ul>

<img src="lambda_pipeline.png" alt="ETL Pipeline Architecture">

<hr>

<h2>Step 1: Create S3 Buckets</h2>
<p>
Create two S3 buckets:
</p>
<ul>
    <li><b>Input Bucket</b> – to upload CSV files</li>
    <li><b>Output Bucket</b> – to store converted JSON files</li>
</ul>

<img src="buckets.png" alt="S3 Buckets">

<hr>

<h2>Step 2: Create AWS Glue ETL Job</h2>
<p>
Create an AWS Glue ETL job that reads CSV data from the input bucket and writes JSON output
to the target bucket.
</p>

<img src="AWS glue  ETL jobs" alt="AWS Glue Job">

<hr>

<h2>Step 3: Define ETL Source</h2>
<p>
Configure the data source in Glue to read CSV files from the input S3 bucket.
</p>

<img src="ETL_source.png" alt="ETL Source">

<hr>

<h2>Step 4: Configure IAM Role for Glue</h2>
<p>
Attach the following policies to the Glue IAM role:
</p>
<ul>
    <li>AmazonS3FullAccess</li>
    <li>AWSGlueServiceRole</li>
    <li>AWSGlueConsoleFullAccess</li>
    <li>AWSLambda_FullAccess</li>
</ul>

<img src="etl_role_iam.png" alt="IAM Role for Glue">

<hr>

<h2>Step 5: Choose S3 Path for Glue Script</h2>
<p>
While creating the Glue job, specify the script location in the following format:
</p>

<code>s3://bucket/prefix/path/</code>

<hr>

<h2>Step 6: Configure Lambda Trigger</h2>
<p>
Create an AWS Lambda function and configure an S3 trigger:
</p>
<ul>
    <li>Event Type: <b>PUT</b></li>
    <li>Suffix: <b>.csv</b></li>
</ul>

<img src="add_trigger.png" alt="Lambda Trigger Configuration">

<hr>

<h2>Step 7: Lambda Function Code</h2>
<p>
The Lambda function triggers the AWS Glue job whenever a CSV file is uploaded.
</p>

<pre>
import json
import boto3

glueClient = boto3.client('glue')

def lambda_handler(event, context):
    glueClient.start_job_run(JobName="csv_to_json")
    return "job started"
</pre>

<p>File name: <b>lambdafun.py</b></p>

<hr>

<h2>Step 8: Lambda IAM Policies</h2>
<p>
Attach the following policies to the Lambda execution role:
</p>
<ul>
    <li>AmazonS3FullAccess</li>
    <li>AWSGlueConsoleFullAccess</li>
    <li>AWSGlueServiceRole</li>
    <li>AWSLambda_FullAccess</li>
    <li>AWSLambdaBasic</li>
</ul>

<img src="lambda_policies.png" alt="Lambda IAM Policies">

<hr>

<h2>Step 9: Pipeline Execution</h2>
<p>
Upload a CSV file to the input S3 bucket. This triggers the Lambda function,
which starts the Glue ETL job.
</p>

<img src="lambda_pipeline.png" alt="Pipeline Execution">

<hr>

<h2>Step 10: Job Completion</h2>
<p>
Once the Glue job finishes successfully, the converted JSON file is stored
in the target S3 bucket.
</p>

<img src="job_done.png" alt="Job Completed">

<hr>

<h2>Output</h2>
<p>
The final JSON output can be found in the target S3 bucket.
</p>

<h3>Project Files</h3>
<ul>
    <li>csv-to-json (Glue ETL script)</li>
    <li>lambdafun.py</li>
    <li>README.html</li>
    <li>Screenshots (*.png)</li>
</ul>

<hr>

<p><b>Result:</b> Automated, serverless ETL pipeline using AWS services.</p>

</body>
</html>
