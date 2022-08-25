# Udacity Nano Degree Devops Projects
## Project 1
### Summary
   The aim of the project was to host a static website in an S3 bucket and cache content with Cloudfront.
### Project Tasks
   * Create a public S3 bucket and Upload the website files to your bucket.
   * Configure the bucket for website hosting and secure it using IAM policies.
   * Speed up content delivery using AWS’s content distribution network service, CloudFront.
   * Access your website in a browser using the unique CloudFront endpoint.
### Project Steps
   1. Create the public S3 bucket from the console and assign it an arbitary name `my-123456789-bucket`.
   2. Upload files:
      ```Bash
      # Create a PUBLIC bucket in the S3, and verify locally as 
      aws s3api list-buckets 
      # Download and unzip the udacity-starter-website.zip 
      cd udacity-starter-website 
      # Assuming the bucket name is my-bucket-202203081
      # Put a single file. 
      aws s3api put-object --bucket my-bucket-202203081 --key index.html --body index.html 
      # Copy over folders from local to S3 
      aws s3 cp vendor/ s3://my-bucket-202203081/vendor/ --recursive 
      aws s3 cp css/ s3://my-bucket-202203081/css/ --recursive 
      aws s3 cp img/ s3://my-bucket-202203081/img/ --recursive 
      ```
   3. Enter the following bucket policy replacing `your-website` with the name of your bucket and click “Save” (**this step is required for static website hosting**):
      ```Bash
        {
         "Version":"2012-10-17",
         "Statement":[
        {
          "Sid":"AddPerm",
          "Effect":"Allow",
          "Principal": "*",
          "Action":["s3:GetObject"],
          "Resource":["arn:aws:s3:::your-website/*"]
         }
         ]
        }
      ```
   4. Configure S3 Bucket for static website hosting from the **Properties tab**.
   5. Distribute website via CloudFront by creating a distribution and assigning the S3 bucket as the origin domain. Paste the static website hosting endpoint e.g.
     ```
     <bucket-name>.s3-website-region.amazonaws.com
     ```
  ---
  ## Project 2
### Summary
   In this project, you’ll deploy web servers for a highly available web app using CloudFormation. You will write the code that creates and deploys the infrastructure and application for an Instagram-like app from the ground up. You will begin with deploying the networking components, followed by servers, security roles and software. 
### Project Tasks
   * Create a **Launch Configuration** for your application servers in order to deploy four servers, two located in each of your private subnets. The launch configuration will be used by an auto-scaling group.
   * You'll need two vCPUs and at least 4GB of RAM. The Operating System to be used is Ubuntu 18. So, choose an Instance size and Machine Image (AMI) that best fits this spec. Be sure to allocate at least 10GB of disk space so that you don't run into issues.
   * Create an **IAM Role** that allows your instances to use the **S3 Service**. Udagram communicates on the default `HTTP Port: 80`, so your servers will need this inbound port open since you will use it with the **Load Balancer** and the **Load Balancer Health Check**. As for outbound, the servers will need unrestricted internet access to be able to download and update their software.
   * The **load balancer** should allow all public traffic `(0.0.0.0/0)` on `port 80` inbound, which is the default `HTTP port`. Outbound, it will only be using `port 80` to reach the internal servers.
   * The application needs to be deployed into private subnets with a **Load Balancer** located in a public subnet. One of the output exports of the **CloudFormation** script should be the public URL of the **LoadBalancer**.
### Project Steps
   1. You can deploy your servers with an **SSH Key** into Public subnets while you are creating the script. This helps with troubleshooting. Once done, move them to your private subnets and remove the **SSH Key** from your **Launch Configuration**.
   2. It also helps to test directly, without the load balancer. Once you are confident that your server is behaving correctly, increase the instance count and add the load balancer to your script.
   3. While your instances are in public subnets, you'll also need the `SSH port open (port 22)` for your access, in case you need to troubleshoot your instances.
   4. Log information for UserData scripts is located in this file: `cloud-init-output.log` under the folder: `/var/log`.
   5. You should be able to destroy the entire infrastructure and build it back up without any manual steps required, other than running the **CloudFormation** script.
   6. The provided UserData script should help you install all the required dependencies. Bear in mind that this process takes several minutes to complete. Also, the application takes a few seconds to load. This information is crucial for the settings of your load balancer health check.
   7. It's up to you to decide which values should be parameters and which you will hard-code in your script.
      
