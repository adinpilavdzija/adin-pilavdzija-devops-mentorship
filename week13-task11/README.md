# 11 AWS Tools GitFlow Workshop

## Task

To complete this task you need to complete AWS Tools GitFlow Workshop available on the following link:  
[AWS Tools GitFlow Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/484a7839-1887-43e8-a541-a8c014cd5b18/en-US/introduction)

**IAM User 1 ce svoje resurse da kreira unutar eu-central-1 regiona.  
IAM User 2 ce svoje resurse da kreira unutar us-east-1 regiona.  
IAM User 3 ce svoje resurse da kreira unutar eu-west-1 regiona**

Please pay attention to the following:

*   You will execute workshop tasks inside your own AWS Account, where you see the part explaining Create an AWS Account (skip that part since you already have an IAM User and AWS Account).
*   Adjust workshop to the region in which you are working, for example when it say [Launch AWS Cloud9 in US-EAST-1](https://catalog.us-east-1.prod.workshops.aws/workshops/484a7839-1887-43e8-a541-a8c014cd5b18/en-US/introduction/getting-started/on-your-own/workspace#launch-aws-cloud9-in-us-east-1:) you should do it in the region which is stated above for your IAM user.
*   **You need to follow steps inside [AWS Cloudformation](https://catalog.us-east-1.prod.workshops.aws/workshops/484a7839-1887-43e8-a541-a8c014cd5b18/en-US/cfn)** (for this task you will not follow AWS CDK Steps)

**Ticket outcomes:**

*   Through this workshop, you should try not just to blindly follow steps but also to analyze and understand the commands you are executing.
*   You should carefully read all posts referenced in this workshop, for example ([https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/) )
*   You should create a directory inside your DevOps Mentorship Program repo and all files used for this task put under one directory and put a pull request as a comment to this ticket.  
    For example:

```
dzenan-dzevlan-devops-mentorship-program
|
|-week-13
       |
       |-all-files-and-dirs-used-for-this-task
       |-README.md (your personal notes)
```

For any questions please feel free to reach out via our slack group #mentorship-program-all.

---

## 2 popular branching models

1. Trunk-based model - developers collaborate on code in a single branch called “trunk” resisting any pressure to create other long-lived development branches by employing documented techniques. This leads to avoiding merging complexity and hence effort. 
2. Feature-based ("GitFlow") model -  involves creating multiple levels of branching off of master where changes to feature branches are only periodically merged all the way back to master to trigger a release. Master always and exclusively contains production code. Develop is the basis for any new development efforts you make. These two branches are so-called long-running branches: they remain in your project during its whole lifetime. Other branches, e.g. for features or releases, only exist temporarily: they are created on demand and are deleted after they've fulfilled their purpose.

### GitFlow guidelines:

- Use development as a continuous integration branch.
- Use feature branches to work on multiple features.
- Use release branches to work on a particular release (multiple features).
- Use hotfix branches off of master to push a hotfix.
- Merge to master after every release.
- Master contains production-ready code.

![image](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/3bcd56bb-bf02-4473-9e7f-a97fed2e3328)

## AWS Cloud9 IDE

AWS Cloud9  is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes prepackaged with essential tools for popular programming languages...

### Resize the Cloud9 instance

```
df -h #reports file system disk space usage in human-readable format
touch resize.sh #kreiranje resize.sh skripte, nakon cega je bilo potrebno kopirati datu skriptu iz dokumentacije; resize.sh se nalazi u repozitoriju
bash resize.sh 30 #pokretanje kreirane skripte i prosljedjivanje parametra 30 za velicinu EBS volumea od 30GiB
```

Initial Setup
```
git config --global user.name "adinpilavdzija"
git config --global user.email adin.pilavdzija@edu.fit.ba
```

```
#configure the AWS CLI credential helper for HTTPS connections
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```

Install gitflow
```
#install gitflow
curl -OL https://raw.github.com/nvie/gitflow/develop/contrib/gitflow-installer.sh
chmod +x gitflow-installer.sh
sudo git config --global url."https://github.com".insteadOf git://github.com
sudo ./gitflow-installer.sh
```

## Elastic Beanstalk Application

Elastic Beanstalk lets you easily host web applications without needing to launch, configure, or operate virtual servers on your own. It automatically provisions and operates the infrastructure (e.g. virtual servers, load balancers, etc.) and provides the application stack (e.g. OS, language and framework, web and application server, etc.) for you.

![image](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/5acf317c-aabc-4cfe-a361-87fedfa4f872)
![image](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/77de819c-e7e9-4138-9e97-db06ad458f33)

### Stage 1: Create Code Commit Repo

```
aws codecommit create-repository --repository-name gitflow-workshop --repository-description "Repository for Gitflow Workshop"
git clone https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/gitflow-workshop
```

### Stage 2: Download the sample code and commit your code to the repository

```
ASSETURL="https://static.us-east-1.prod.workshops.aws/public/442d5fda-58ca-41f0-9fbe-558b6ff4c71a/assets/workshop-assets.zip"; wget -O gitflow.zip "$ASSETURL" #Download the Sample App archive by running the following command from IDE terminal.

unzip gitflow.zip -d gitflow-workshop/ #Unarchive and copy all the contents of the unarchived folder to your local repo folder

#Change the directory to your local repo folder. Run git add to stage the change.
cd gitflow-workshop
git add -A

#Run git commit to commit the change and push it to master
git commit -m "Initial Commit"
git push origin master
```

### Create Elastic Beanstalk Application

In Elastic Beanstalk an application serves as a container for the environments that run your web app, and versions of your web app's source code, saved configurations, logs, and other artifacts that you create while using Elastic Beanstalk.

Run the following AWS CloudFormation template to create
- Elastic Beanstalk application - think of it as a folder that will hold the components of your Elastic Beanstalk
- S3 bucket for artifacts - place to put your application code before deployment

```
aws cloudformation create-stack --template-body file://appcreate.yaml --stack-name gitflow-eb-app #pokretanje datog CloudFormation templatea za kreiranje Elastic Beanstalk application i S3 bucket for artifacts 
```

## Master Environment

### Creating an AWS Elastic Beanstalk Master Environment

You can deploy multiple environments when you need to run multiple versions of an application. For example, you might have development, integration, and production environments

Potrebno je promijeniti vrijednost za SolutionStackName u envcreate.yaml fajlu:
```
SolutionStackName: 64bit Amazon Linux 2 v5.8.1 running Node.js 16 DA
```

Potrebno je i kreirati IAM rolu sa [tutorijala](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/iam-instanceprofile.html) zbog:  
_Previously Elastic Beanstalk created a default EC2 instance profile named aws-elasticbeanstalk-ec2-role the first time an AWS account created an environment. This instance profile included default managed policies. If your account already has this instance profile, it will remain available for you to assign to your environments.  
However, recent AWS security guidelines don’t allow an AWS service to automatically create roles with trust policies to other AWS services, EC2 in this case. Because of these security guidelines, Elastic Beanstalk no longer creates a default aws-elasticbeanstalk-ec2-role instance profile._

Pa tek onda izvrsiti komandu:
```
aws cloudformation create-stack --template-body file://envcreate.yaml --parameters file://parameters.json --capabilities CAPABILITY_IAM --stack-name gitflow-eb-master
```

### AWS CodePipeline

AWS CodePipeline is a continuous delivery service you can use to model, visualize, and automate the steps required to release your software.

![image](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/bae699be-0222-4620-9a1e-ff04422b776d)

Potrebno je u fajlu buildspec.yml verziju nodejs postaviti na 14.

## Lambda

AWS Lambda is a compute service that lets you run code without provisioning or managing servers. AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second. You pay only for the compute time you consume - there is no charge when your code is not running. With AWS Lambda, you can run code for virtually any type of application or backend service - all with zero administration.

![image](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/84d44735-3ead-4e3d-9dce-45cf4af826d0)

### Create Lambda

U fajlu lambda-create.yaml promijeniti policy iz AWSCodePipelineFullAccess u AWSCodePipeline_FullAccess. U istom fajlu potrebno je za S3Bucket postaviti naziv naseg bucketa. Potrebno je postaviti i S3Key. Na edit default encryption za taj bucket izaberemo KMS - aws/s3 kljuc, key arn kopiramo u fajl, zipujemo i uploadujemo na bucket). Za S3Key postavimo naziv tog uploadovanog zip fajla.

```
aws cloudformation create-stack --template-body file://lambda/lambda-create.yaml --stack-name gitflow-workshop-lambda --capabilities CAPABILITY_IAM
```

### AWS CodeCommit Trigger

You can configure a CodeCommit repository so that code pushes or other events trigger actions, such as sending a notification from Amazon Simple Notification Service (Amazon SNS) or invoking a function in AWS Lambda. You can create up to 10 triggers for each CodeCommit repository.

Triggers are commonly configured to:
- Send emails to subscribed users every time someone pushes to the repository.
- Notify an external build system to start a build after someone pushes to the main branch of the repository

## Develop Branch

```
git flow init #initialize gitflow
git branch #List the current branches. The current branch will be highlighted in green and marked with an asterisk.
git push -u origin develop #Push commits made on a local branch to a remote repository
```

### Create Development Environment

After pushing the commits to development branch, the lambda function you created earlier will automatically detect a new develop branch created and codecommit will trigger the function to create the development environment and code pipeline.
```
aws cloudformation create-stack --template-body file://envcreate.yaml --parameters file://parameters-dev.json --capabilities CAPABILITY_IAM --stack-name gitflow-workshop-develop
```

## Feature Branch

Each new feature should reside in its own branch, which can be pushed to the central repository for backup/collaboration. But, instead of branching off of master, feature branches use develop as their parent branch. When a feature is complete, it gets merged back into develop. Features should never interact directly with master.

The idea is to create a pipeline per branch. Each pipeline has a lifecycle that is tied to the branch. When a new, short-lived branch is created, we create the pipeline and required resources. After the short-lived branch is merged into develop, we clean up the pipeline and resources to avoid recurring costs.

### Create feature branch

```
git flow feature start change-color #kreira branch feature/change-color iz develop brancha i vrsi checkout na feature/change-color branc
```

Commit a change and then update your app
```
git status    
git add -A  
git commit -m "Changed Color" 
git push --set-upstream origin feature/change-color
```

### Feature Finish

Once you have verified the changes you made and is ready to merge to develop branch.
```
git flow feature finish change-color #Merges change-color into develop; Removes the feature branch; Switches back to develop branch
git push origin --delete feature/change-color #Delete the feature branch change-color and push it to remote at the same time
git push --set-upstream origin develop #Push the develop branch changes to codecommit
```
<hr>

Na kraju workshopa je izvrsen cleanup.