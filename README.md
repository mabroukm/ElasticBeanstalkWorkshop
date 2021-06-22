# AWS Elastic Beanstalk for .Net Apps Workshop
AWS Elastic Beanstalk for .Net Apps Workshop is an extension to '.Net on AWS ImmersionDay' that aims at covering common usecases for running .net apps on AWS Elastic Beanstalk service.

Through this set of labs, you will try to
- Create, clone and configure Elastic Beanstalk environments
- Deploy multiple Dotnet apps to the same Elastic Beanstalk environment.
- Build a CI/CD pipeline that deploys your apps to AWS Elastic Beanstalk.
- Setup monitoring dashboard for your environment.

Let's get started.

## Setup your development tools
<details>
<summary>Click to expand</summary>
<br/>   

   Go to [Lab Login](https://dashboard.eventengine.run/login) and enter in the code given to you to get started with your account for the labs.

> Note: If you already have an AWS account, open the above link in incognito/private mode so that you don’t accidently make changes to your AWS account.


You can run these labs using tools on you local machine or by running them on an EC2 instance.

### Use an EC2 Instance
<details>
<summary>Click to expand</summary>
   
  [Click here](https://console.aws.amazon.com/cloudformation/home#/stacks/new?region=ap-southeast-2&stackName=WIN314Stack&templateURL=https://immersiondaypublicdatabucket.s3.amazonaws.com/Main-Dev-Env-EC2-CFN-2020-07-23-Immersion-Day.yml) to deploy the Dev Box to your account.

1. Review to ensure the template has a source of Amazon S3 URL and the URL is set as input and click Next

2. On the **Specify stack details** page, change the following parameters
  
  |Parameter|New Value|
  | ----------- | ----------- |
  |UseDefaultVPC|true|
  |LabGuideUrl|https://github.com/mabroukm/ElasticBeanstalkWorkshop|
  |BootstrapCDK||
  |SampleAppGitRepoUrl|https://github.com/mabroukm/ElasticBeanstalkWorkshop.git|
  |SampleAppSolutionDir|./source/repos|
  |SampleAppCodeCommitRepoName|ElasticBeanstalkWorkshop|
  |LinuxDockerInstanceSize||
  |RipEcrRepoName||
  
  Review the other parameters then Click Next

3. On the “Configure stack options” page, take the defaults and click Next

4. Finally, on the Review screen, scroll to the bottom of the page and you will see a “Capabilities” box. Check the checkbox next to all of the acknowledgements and click Create Stack

5. This brings you to the CloudFormation console page

  a. Check the box next to your Stack Name to see its details.

  b. If your Stack Name is not displayed, click the refresh button (circular arrow) in the top right until it appears.

  c. If the details are not displayed, click the refresh button until details appear.

Choose the Events tab for your selected workload to see the activity log from the creation of your CloudFormation stack. Wait for it to say **CREATE_COMPLETE**

   ![AWS CloudFormation Console](/images/setup_01.png)
   
   
Now let’s RDP into your dev machine.

1. Start by navigating to the EC2 Dashboard and click on Instances (running).
![EC2 Console](/images/setup_02.png)
2. Select the server with name “Workshop - .NET development on AWS”, and click Connect on the top menu bar.
![EC2 Console - Instance details](/images/setup_03.png)
3. On the “Connect to Instance” page, select RDP Client and click Download remote desktop file. You do not need to click Get Password. The password will be provided to you in a later step.
   
4. Launch the RDP session by opening the downloaded file.

5. When you are prompted for credentials first click on More choices, then click on Use a different account and then enter the following credentials:
   ```
   username:  .\Administrator  
   password:  ImmersionDayW0rkshop+TheStrong1
   ```
  ![RDP Client](/images/setup_04.png)
> Note: you do not need to install the AWS Toolkit, it is installed already on this development instance.
</details>

### Run on your local machine
<details>
<summary>Click to expand</summary>
   
   Make sure that you have instealled the tools in the below list before you move to the next step
   |Tool|Version|
   | ----------- | ----------- |
   |Visual Studio|2019 Community or Enterprise|
   |AWS Toolkit for Visual Studio 2017 and 2019|1.21.2.0|
   |Git|2.30.x|

</details>
   
 Now let's create a user to use from Visual Studio
 ### Setting up IAM user
 <details>
<summary>Click to expand</summary>
 1. Now, you will want to create a new IAM User so that you can access your AWS resources through programmatic access. In the AWS console, under Services select IAM.

2. On the left hand side of the screen, click Users

3. Click Add user. Give the user a username like VSDev, and check the checkbox for Programmatic access under Access type
    ![IAM Console - Add User](/images/setup_05.png)
4. Click Next: Permissions

5. Select “Attach existing policies directly” at the top of the screen, then select AdministratorAccess under the list of policies
    ![IAM Console - Add User - Attach existing policies directly](/images/setup_06.png)
6.Click Next: Tags, then click Next:Review
    
7. Click Create user. This will send you to a screen that shows you both the Access Key and Secret Access key that you just created that is linked to the new user. It also provides you the option to download the credentials.
    
8. Click Download .csv Save the file somewhere you can find as we will use this file to import the access keys in a later step.
   ![IAM Console - Download Credentials](/images/setup_07.png) 
    
 </details>    
   
### AWS Toolkit for Visual Studio profile setup
<details>
   <summary>Click to expand</summary>
   
   In this section, we will be adding account credentials to your toolkit to allow you to interact with AWS services from within Visual Studio.
1. Start Visual Studio. If this is the first time launching Visual Studio after installing the AWS toolkit and no other credential profiles exist on your system it will display the AWS Getting Started view inviting you to add credentials.
   ![Getting Started with AWS Toolkit for Visual Studio](/images/setup_08.png)
> Note: If the AWS Getting Started view does not display (for whatever reason) you can still add a new credential profile using the AWS Explorer window, as follows
   
    a. Open the AWS Explorer window by selecting View > AWS Explorer from the main menu.
   
    b. Click the New account profile button to the right of the Profile field (the first button in the set of three).
   
    c. The New Account Profile dialog is displayed, as shown
   
<img src="/images/setup_09.png"></img>
   
    d. You can now resume with the instructions below which apply to either window.

2. Enter a name for the credential profile. This can be the same name as the IAM user you created or you can use default, as suggested in the dialog. If you use the name default the tools will locate and use it automatically if no other credential profile is specified.
   
> Note: if you elect to use a custom name you will need to specify the profile name when using the dotnet CLI extensions in later modules using the –profile option. All instructions and screenshots in this guide assume you have named your credential profile default.
 
3. Use the csv credentials file that was downloaded in the pre-requisites “Create Visual Studio Environment” steps.

4. Click the Import from csv file button, navigate to the csv file you downloaded in the previous step and select it before clicking OK to close the dialog.

5. The access and secret access keys for the user will be loaded into the view.

6. You may leave the Account Number blank if you wish. For all standard public AWS accounts leave the Account Type field at Standard AWS Account. If you are using an AWS GovCloud account, or are in the AWS China region, select the correct account type in the field.

7. Click Save and close button (OK in the New Account Profile window) to close the view. Your new credential profile will be preselected in the AWS Explorer window ready for use.

</details>   
   
   Now let's start building.
</details>

## Deploy an ASP .Net Framework to AWS Elastic Beanstalk from Visual Studio
<details>
<summary>Click to expand</summary>

1. If you use the AWS EC2 Instance dev box, you should be able to have the below solution in C:\Users\Administrator\source\repos folder. If you cannot find it for any reason or you are using your own machine to run the lab you can pull the solution from this repo https://github.com/mabroukm/ElasticBeanstalkWorkshop.git

   ![VS - Solution Explorer](/images/eb-from-vs-01.png) 

2. From **Solution Explorer** view right click on DotnetFrameworkASPWebApp project and select Publish to AWS Elastic Beanstalk...

3. **Create a new application environment** option will be automatically selected 

   ![VS - Solution Explorer](/images/eb-from-vs-02.png) 
   
4. On the **Application Environment** window, enter or select from the drop down list Application and Environment names. You add your name initials to get an available URL value. Click Next

   ![VS - Solution Explorer](/images/eb-from-vs-03.png)
   
5. Review and accept default values on this screen. Before you click next, take your time to understand what are all these values for.
**Key pair** is important if you would like to access the EC2 instances that Elastic Beanstalk will create on your behalf.

   ![VS - Solution Explorer](/images/eb-from-vs-04.png)
   
   
6. Review and accept the default values here too. Do you know what is X-Ray? It is an AWS tool that helps developers to analyse and debug production applications.

   ![VS - Solution Explorer](/images/eb-from-vs-05.png)

   
7. Review all settings then click Deploy.

   ![VS - Solution Explorer](/images/eb-from-vs-06.png)

   
8. Now go to AWS Elastic Beanstalk console [here](https://ap-southeast-2.console.aws.amazon.com/elasticbeanstalk/home?region=ap-southeast-2#/environments). Click on the URL to open your app.

   ![VS - Solution Explorer](/images/eb-from-vs-07.png)
   
9. Get back to Visual Studio and browse to **_Home.cshtml** page under **Views** folder and change line 9 to 
   ```
           <h2>Elastic Beanstalk Workshop V2</h2>
   ```
   
10. Build the project and redeploy the project again. While redeploying go to your app URL and refresh it. Did you notice that your app is down for few minutes? That could be acceptable for some apps. You may also consider deploying outside operation hours. But what if your business cannot tolerate that downtime?
   
11. Go back to AWS Elastic Beanstalk console [here](https://ap-southeast-2.console.aws.amazon.com/elasticbeanstalk/home?region=ap-southeast-2#/environments), from **Configuration** page on the left hand side of the page, select **Edit** button in **Rolling updates and deployments** section

   ![VS - Solution Explorer](/images/eb-from-vs-09.png)   

12. Update the Deployment Policy to **Immutable** then go back Visual Studio and redeploy. Watch the application URL. What did you notice this time? There was almost no noticable downtime. That is because Elastic Beanstalk create a new scaling group and put it into service then removes the old scaling group.
   
   ![VS - Solution Explorer](/images/eb-from-vs-10.png) 

There are two other Deployment Policies that are not in the droplist items; they are **Rolling** and **Rolling with additional batch**. Ask the solutions architect about them and why they are hidden or have a read [here](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rollingupdates.html)
   
What if we need to deploy multiple .Net apps to the same Elastic Beanstalk Environment? Please move to the next section to see how does that work.

</details>

## Deploy Multiple .Net Apps to the same Elastic Beanstalk Environment
<details>
<summary>Click to expand</summary>
   
   AWS Toolkit for Visual Studio doesn't support deploying multiple apps to Elastic Beanstalk, the apps need to be packaged manually. The way that works is that we will publish both projects to file system then archive them together with a manifest file that describes how those apps will be deployed to IIS. Let's that package that together.
   
1. Right click on **DotnetFrameworkASPWebApp** project from **Solution Explorer** and choose **Publish**. On **Publish** window, select Target **Web Server(IIS)**. Click Next
   
   ![VS - Solution Explorer](/images/eb-manual-01.png)

2. For Specific taget, select **Web Deploy Package**
   
   ![VS - Solution Explorer](/images/eb-manual-02.png)
   
3. For IIS Connection, specify Package location to a folder on your desktop. Set **Site name** to **DotnetFrameworkASPWebApp**. Click Finish
   
   ![VS - Solution Explorer](/images/eb-manual-03.png)
   
4. Repeat the above steps for **DotnetWebAPI** to publish it to the same folder.
   
5. Now copy **aws-windows-deployment-manifest.json** file from the solution directory to the package folder. You can also find the file [here](https://raw.githubusercontent.com/mabroukm/ElasticBeanstalkWorkshop/master/aws-windows-deployment-manifest.json). Take a moment to read the file and understand the structure. Please ask the solutions architect if you have any questions.
   
6. From inside the package folder, select all files and compress. The result file is deployable to Elastic Beanstalk.

7. Go to AWS Elastic Beanstalk console [here](https://ap-southeast-2.console.aws.amazon.com/elasticbeanstalk/home?region=ap-southeast-2#/environments) and select your environment then select **Upload and deploy** button from your environment home page.
   
   ![VS - Solution Explorer](/images/eb-manual-04.png)
   
8. Choose the package file and deploy it. Wait until the deployment is complete then append **/website** and **/webapi** to your app URL to access both apps.
   
</details>
