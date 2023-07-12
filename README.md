# TASK - Deploy Django App using AWS Code Pipeline

### Create an EC2 server.
### Pick any Django or python-based sample app from google or create it on your own.
### Deploy that on the EC2 server and use Nginx as a web server.
### Once the site is up and running create a GitHub repository and push the code into that repository.
### Create an AWS Code Pipeline that will build the code and deploy it on the EC2 server.
### Attach an Elastic IP to the EC2 instance and apply the domain and SSL.

## ADDITIONAL TASK
### Once the Pipeline start working properly through Github, deploy it using code commit.
### Attach load balancer and auto-scaling to it.

# Definition Of Done!

### The site should be accessible with the IP on port 80.
### whenever any changes would be made in the code then the pipeline will trigger automatically and deployment should happen on the server.
### Explore how the deployment can be done with zero downtime.

``` Step 1  Create an EC2 server ```

Login to AWS Management Console and open EC2 service.

You can either go to Services -> Compute -> EC2 or type EC2 in the search bar and hit enter. Once you see EC2 option click on that.

This will lead you to EC2 dashboard like below.

To Create EC2 Instance in AWS, Click Launch Instance

Create/Select a Key Pair - Select Create a new key pair option – > name your key pair -> Download Key Pair -> Launch Instance , Download keypair and click Launch Instances

![image](https://user-images.githubusercontent.com/67600604/171338086-a56c4cbd-a8f0-439e-9685-ba2dcd10361b.png)

``` Step 2  Pick any Django or python-based sample app from google or create it on your own and  Deploy that on the EC2 server and use Nginx as a web server ```

I am picking up the default Sample app from the Django and followed this simple article to install the Django application using the gunicorn and done the configuration to run this on port 80

![image](https://user-images.githubusercontent.com/67600604/174781038-62c131a4-4a28-4d99-95b1-95c658977dc0.png)

then pushed all the code files to the github 

``` Step 3 Create an AWS Code Pipeline that will build the code and deploy it on the EC2 server ```

Firstly, create 2 roles in your IAM , one for the EC2 and another one is for the Code deploy, give permmissions accordingly, and attach the ec2 role to your working EC2 Instance 

![image](https://user-images.githubusercontent.com/67600604/174781797-25244da1-1bf7-4ef8-a36b-9c96d887e2fb.png)

![image](https://user-images.githubusercontent.com/67600604/174781875-9120c2ce-fbd5-4378-95ca-517bd945ce66.png)

Image 1 is of the code deploy role and 2nd is of the EC2 role and attach your IAM role

![image](https://user-images.githubusercontent.com/67600604/174782037-a7596cd8-93c1-4760-9d7c-77e027c59e69.png)

Now, run these Commands on your EC2 Instance to install the codedeploy-agent 

```sudo apt-get update
sudo apt-get install awscli
sudo apt-get install ruby2.0
cd /home/ubuntu
sudo aws s3 cp s3://bucket-name/latest/install . --region region-name sudo chmod +x ./install
sudo ./install auto
And check the status of the codedeploy agent. it should be in active state.
```

Now, we will create a codepipeline for the same.

![image](https://user-images.githubusercontent.com/67600604/174783022-d0ed55d3-0d2c-4318-b631-a1f0442cd8af.png) Write the Name of your application you have created in the code deploy 

Let’s navigate to CodePipeline via AWS Management Console and click on Create pipeline. (A new service role for your new pipeline will be created on this page as well. We can let AWS create a new S3 bucket to store artifacts or if you would like to store artifacts on an existing S3 bucket in the same region, you can select “Custom location” and then select your preferred S3 bucket)

And connect your pipeline to the github and enter the repository name that you have done on your github account. 

Now create a appspec.yml file for your application, for me it is on my this github repo and then start the pipeline 

![image](https://user-images.githubusercontent.com/67600604/174783555-db7f7467-6d0a-4f15-9d28-2b71a0348ed0.png)

This status should be successed

``` Step 4 Attach an Elastic IP to the EC2 instance and apply the domain and SSL. ```

Purchase a Elastic IP for your EC2 instance and mention that IP with your Domain in your domain name registerer to target it to the Domain. I am using the freenom.com here and the purchased domain is hitting the url that i have purchased

![image](https://user-images.githubusercontent.com/67600604/174784018-4a31674f-9d5d-40bf-b770-fd9affce838d.png)

``` and for the SSL - https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04 ```

![image](https://user-images.githubusercontent.com/67600604/174784310-557f9ccf-eb87-4c04-93f3-1c2fdf56a43b.png)

## ADDITIONAL

``` Step 1 – Pushed the Same Github code in code deploy ``` 
Create a Repository  in code commit and from the github as we done before, clone the same in code in code commit using the git command and for the authentication for the code commit, 
``` Go into the IAM -> Users -> Security Credentials -> Git Credentials for AWS code Commit ```  
Get the credentials for the code commit to push the code into the newly created repo.

![image](https://user-images.githubusercontent.com/67600604/175942111-9ed0d09f-0990-4e93-bc5d-2ca731aa8817.png)

Then Create a pipeline as the same way we created for github, we just have to do one change is -
Go to the Console create the pipeline , and choose the option code commit in the source stage instead of github and then enter the code commit repo details.
![image](https://user-images.githubusercontent.com/67600604/175942308-bd94d0b3-ea32-4099-989a-2eba2fcc43a3.png)

Now, release the changes in the pipeline as we done in the github case.
```
Note – If facing any kind of error while deploying that is related to the path not found, once check that is the code is getting into the S3 or not. If not, try to do with a freshly created Deployment group, It should work
```

Make some changes in the code commit repo it should automatically trigger the pipeline and check if the changes occurs in the server or not.

![image](https://user-images.githubusercontent.com/67600604/175942505-01647a76-5592-41fc-b46f-d3984a0bd1cd.png)

``` Step 2 – Attach load balancer and auto-scaling to it ```

Create the AMI from your instance so that we can use that image to create more instance while auto scaling 
Go to the AWS console -> EC2 -> target group 
Create the target group for your instance’s AMI, select  the AMI you created and then go to the Load balancer and attach the target group we created from the AMI of our instance.

![image](https://user-images.githubusercontent.com/67600604/175942614-ea00bf7f-9dd1-425d-9d3b-9db69e45e90d.png)

With the help of this check if your load balancer is working or not

![image](https://user-images.githubusercontent.com/67600604/175942669-555c2253-9f04-4fad-99f8-239b03af7f2c.png)

Then go to the target group and check the instance state is healthy or unhealthy

![image](https://user-images.githubusercontent.com/67600604/175942739-06989083-fc16-4a1b-b775-f0a2c8cf1da9.png)

Now Go to the Auto scaling, and go to the launch configuration and click on create launch configurations

![image](https://user-images.githubusercontent.com/67600604/175942782-a47b5dcb-5f29-4ab9-99e1-d1f55325dc72.png)

Enter the details of your roles, IAM instance image and key pair info so that while creating the new instances, it can use those info to create the instances.

![image](https://user-images.githubusercontent.com/67600604/175942819-54947418-defc-4c1e-8289-40cd5039af91.png)

And then go to the Autoscaling and chose the configuration template that we created earlier in the configuration and enter the number of instance you needed to auto scale.

![image](https://user-images.githubusercontent.com/67600604/175942895-1dc3454b-32de-48a7-82e8-5cfa968a431a.png)

![image](https://user-images.githubusercontent.com/67600604/175942921-b0823376-ec99-4d34-a06a-d4290298d613.png)

Here, I have taken three instance, wait to get the instance prepared and now in the target group check the state of all the instances

![image](https://user-images.githubusercontent.com/67600604/175942972-33bf443b-9cdd-41ef-bce2-51062e55870b.png)

All the instance are in the healthy state. edit in the autoscaling to 1 or 4 to check if it is working properly

THANK YOU 
SHRUTI :)
