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
> Note: If the AWS Getting Started view does not display (for whatever reason) you can still add a new credential profile using the AWS Explorer window, as follows:
a. Open the AWS Explorer window by selecting View > AWS Explorer from the main menu.
b. Click the New account profile button to the right of the Profile field (the first button in the set of three).
c. The New Account Profile dialog is displayed, as shown:
   ![Getting Started with AWS Toolkit for Visual Studio - New Account Profile](/images/setup_09.png)
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


