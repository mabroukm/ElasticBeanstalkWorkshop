# AWS Elastic Beanstalk for .Net Apps Workshop
AWS Elastic Beanstalk for .Net Apps Workshop is an extension to '.Net on AWS ImmersionDay' that aims at covering common usecases for running .net apps on AWS Elastic Beanstalk service.

Through this set of labs, you will try to
- Create, clone and configure Elastic Beanstalk environments
- Deploy multiple Dotnet apps to the same Elastic Beanstalk environment.
- Build a CI/CD pipeline that deploys your apps to AWS Elastic Beanstalk.
- Setup monitoring dashboard for your environment.

Let's get started.

## Setup your development tools

Go to [Lab Login](https://dashboard.eventengine.run/login) and enter in the code given to you to get started with your account for the labs.

> Note: If you already have an AWS account, open the above link in incognito/private mode so that you don’t accidently make changes to your AWS account.


You can run these labs using tools on you local machine or by running them on an EC2 instance.

### Use an EC2 Instance
<details>
<summary>Click to expand</summary>
   
  [Click here](https://console.aws.amazon.com/cloudformation/home#/stacks/new?region=ap-southeast-2&stackName=WIN314Stack&templateURL=https://immersiondaypublicdatabucket.s3.amazonaws.com/Main-Dev-Env-EC2-CFN-2020-07-23-Immersion-Day.yml) to deploy the Dev Box to your account.

1. Review to ensure the template has a source of Amazon S3 URL and the URL is set as input and click Next

2. One the **Specify stack details** page, change **only** the following parameters
  
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
  
  Click Next

3. On the “Configure stack options” page, take the defaults and click Next

4. Finally, on the Review screen, scroll to the bottom of the page and you will see a “Capabilities” box. Check the checkbox next to all of the acknowledgements and click Create Stack

5. This brings you to the CloudFormation console page

  a. Check the box next to your Stack Name to see its details.

  b. If your Stack Name is not displayed, click the refresh button (circular arrow) in the top right until it appears.

  c. If the details are not displayed, click the refresh button until details appear.

Choose the Events tab for your selected workload to see the activity log from the creation of your CloudFormation stack. Wait for it to say CREATE_COMPLETE
  
  
  
</details>

### Run on your local machine
<details>
<summary>Click to expand</summary>

</details>

