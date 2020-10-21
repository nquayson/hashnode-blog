## The cloud resume challenge was worth its weight in gold!

> It has been one exciting project in which I have learned to put together several AWS Services and skills that I picked up along my AWS SAA certification journey. 

Around the beginning of this month, a good friend introduced me to  [Forrest’s cloud resume challenge](https://cloudresumechallenge.dev/halloffame/). It challenges participants to build a serverless resume website using AWS cloud infrastructure; building and configuring services using cloud IaC automation in a CICD Pipeline. 


Found it at a time that I had newly passed the AWS Solutions Architect – Associate exam, and it presented a great opportunity to test myself with a practical problem, have fun practicing with it, and showcase my skills.  

Prior to this project, I had had some experience with relational database instances running on-prem/server and working on some few small python projects. But I had never fully performed a CICD Pipeline implementation for a lot of the cloud services that were required for this challenge. The more I dived deep, the more my interest heightened. I, perhaps, spent a lot more time reading (because of my curiosity) than actually working on the project. 

In a nutshell, the steps that were taken to complete the challenge, in no specific order, involved: creating the frontend and then hosting it in S3, Domain registration and DNS configuration, AWS SAM and CloudFormation for building the serverless backend stack; which was comprised of AWS API Gateway + Lambda (Python) + DynamoDB, then finally, continuous integration and deployment using Github Actions. My codebase was stored on Github and I incorporated Unit Tests into my CICD pipeline. I started straight out taking the bull by the horns, from the backend, using a small dummy html file.

# Backend
I built a small Linux (Ubuntu) box at home exclusively for the project. I installed and setup Python, PIP, Boto3, AWS CLI, SAM CLI, Docker, DynamoDB Local. I did this to provide me with the flexibility to try out features locally, before meddling with resources in the cloud. 
First, I experimented with basic CRUD in DynamoDB Local using Python and Boto3. This was my first real experience with a NoSQL DB. It was fairly easy to pick up. I also experimented with provisioning resources in the cloud from my local linux CLI. I ended up creating multiple AWS SAM templates for my backend infrastructure which, again, comprised of API Gateway, Lambda Function Event, and DynamoDB. Running from the AWS CLI and AWS SAM CLI, I succeeded with deploying my backend resources. Next task was to automate provisioning these same resources in a CICD Pipeline. 

# Automation and CICD Pipeline
I have basic knowledge of CICD but had never implemented one. I did some extensive reading around AWS CodePipeline, CircleCI and Github Actions. AWS CodePipeline looked like the easiest for this task, but I wanted to try something new. 
I settled on GitHub actions, which was released a couple of months ago. My Actions workflow run in a Linux Docker container, mostly executing bash scripts. Environment variables such as access keys were securely set in Github Secrets. 
For the frontend repository, on each push of my local code to GitHub, the actions CICD is triggered and the code base is automatically synced with the hosting S3 bucket. Then the CloudFront cache is invalidated. 
The workflow for the backend CICD pipeline involved Setting up the job, Checkout, Installing SAM CLI, Unit Testing, SAM Build, Deploy, and Checkout. SAM template Code is only built and deployed after the Python Lambda function passed the Unit tests. 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/7q7u8v59fh563ardmchp.PNG)

# Frontend
At this point, I was confident I had completed a bulk of the tasks required. I have basic HTML and CSS knowledge and have not built an entire website all by myself. I usually find it comfortable working in the backend. It was challenging to find a website template that I liked exactly. So, I completely stripped apart a minimal html blog template which I had found on HUGO; modified it to look like a resume (I spent lots of hours trying to make it look elegant ++insert smiling face++). I did a simple JS XHR GET request to the API to get the visitor count. This step helped brush up my rusty CSS skills and reminded me to appreciate design work, even more. 

# DNS, Domain, Hosting, CDN, SSL Certificate
I realize people like to keep things within the AWS ecosystem for simplicity, but it was not too hard to apply my DNS knowledge to hook my CloudFront distribution with an external domain registrar. 
There were a few downsides with AWS Certificate Manager during DNS validation using the CName record, but I was able to get it fixed by googling. I also had to switch my AWS region to us-east-1 to use ACM according to AWS FAQ. 

# Final Words
This project presented a refreshing challenge, motivating me to do a lot of reading about Docker and the AWS CLI. I have learnt CICD automation for cloud infrastructure and how to include unit testing into the workflow Pipeline. I learned to integrate a lot of the services, which I had previously not used together. I discovered portals where I can do further reading when I run into problems with AWS services. Big thanks to Forrest Brazeal for allowing me the opportunity to take part in this challenge. 
Here is the [end product](https://nquayson.com). And here are my [github notes](https://github.com/nquayson/aws-solutions-architect-associate-notes) for the AWS Solutions Architect – Associate exam. 