In this blog, I will discuss how to build Host a static website using AWS S3, CloudFront and enabling CICD Pipelines. I will also cover how to automate the end-to-end deployments using a continuous integration and deployment (CICD) pipeline. 

I opted to use AWS S3 with CloudFront because it offers significant advantages for hosting static websites like Scalability, Cost, Durability, Performance, Caching, and Certificate management.Let’s get started.

**STEP-1: SOURCE CODE MANAGEMENT**
Create a git repo in Github, BitBucket or any other source control management system of our choice. 

**STEP-2: BUILD YOUR SITE**
I have used index.html template from w3 schools for a start. You can use your own content. I'll gradually update my website.

**STEP-3: **CREATE A S3 BUCKET**
Create a S3 static website to host website. The name of the bucket should match your domain name e.g. cornersquare.tk.

S3 bucket should publicly accessible and static website property should be enabled.

You can make the S3 bucket publicly accessible with all it’s content (use with caution) using this bucket policy:

{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
{
            "Sid": "1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity E3OCXNAWBI8FTI"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cornersquare.tk/*"
        },
        {
            "Sid": "2",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity E3OCXNAWBI8FTI"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cornersquare.tk/*"
        },
        {
            "Sid": "3",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity E3OCXNAWBI8FTI"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cornersquare.tk/*"
        }
    ]
}

**STEP-4: AWS CERTIFICATE MANAGER (ACM) FOR CERTIFICATES**
Create a HTTPS/SSL certificate for the domain using AWS Certificate Manager (ACM). There are some restrictions around regions, so my recommendation would be to select us-east-1 region for CloudFront compatibility.

**STEP-5: CLOUDFRONT FOR CONTENT DELIVERY NETWORK**
Create a CloudFront distribution pointing, for example

Origin: Should point to S3 bucket endpoint. 
Root: index.html
Domain: cornersquare.tk, www.cornersquare.tk
Viewer Protocol Policy: Redirect HTTP to HTTPS
Select rest of the configurations based upon your requirements.

SERVE INDEX.HTML FROM SUB-DIRECTORY
To ensure that index.html files are served correctly from sub-directories, please make sure the origin points to S3 website endpoint (e.g. cornersquare.tk.s3.us-east-1.amazonaws.com) and do not auto-select it from the drop-down. You can get the S3 website endpoint by selecting your bucket and navigating to property section.

**STEP-6: FREENOM TO HOST DNS**
I used freenom as a domain host for my webiste. However, you can use applications like Go Daddy, Namecheap, Route 53 to host your domain name. Create an A record (CNAME) and point it to the CloudFront distribution.

**STEP-7: AWS CODEPIPELINE TO ENABLE AUTOMATIC DEPLOYMENTS**
- I Created a new CICD pipline (give pipeline name, service role as default, create a new service role and assign required permissions). 
- Source stage: Choose GitHub(version 2) as a source provider, connection string, repository name, and the branch. In order to connect to Github, choose Github if you already have an active profile and enter the connection name or else create new.
- Build stage: The build stage is not required (choose skip), as we are going to deploy a static website to S3 bucket.
- Deploy stage: Now choose deploy provider as S3 and region where you want your static website to be hosted. Enter the bucket name and check on Extract file before deploy.
- Review all the changes and create pipeline
- You can see the source pipeline execution is succeeded and your Github Version 2 and Amazon S3 is succeeded.

**STEP-8: TRIGGER PIPELINE**
Push a commit to Git repository. This should auto-trigger the CICD pipeline which will build the site and deploy it to S3.

**STEP-9: LAUNCH STATIC SITE USING AWS SITE USING S3 AND CLOUDFRONT**
Open a browser and you should be able to access the site now. Congrats for launching your new static site using a AWS S3 distributed using CloudFront and deployed using CICD Pipeline (Codedeploy & Github). Announce it to the world by sharing the link on social sites and via other marketing channels.

Hope you enjoyed this blog. Please comment and let us know.
