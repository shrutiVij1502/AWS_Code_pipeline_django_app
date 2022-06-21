# TASK

### Create an EC2 server.
### Pick any Django or python-based sample app from google or create it on your own.
### Deploy that on the EC2 server and use Nginx as a web server.
### Once the site is up and running create a GitHub repository and push the code into that repository.
### Create an AWS Code Pipeline that will build the code and deploy it on the EC2 server.
### Attach an Elastic IP to the EC2 instance and apply the domain and SSL.

# ACCEPTANCE CRITERIA

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

Here is the Article Link - https://www.codewithharry.com/blogpost/django-deploy-nginx-gunicorn , Now our app is working on port 80 , On Nginx server

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


THANK YOU 
SHRUTI :)
