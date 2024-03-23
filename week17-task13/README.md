# 13 AWS Code Family Workshop

## Task 

Potrebno je odraditi navedene Lab-ove iz sljedeceg [workshop-a](https://catalog.us-east-1.prod.workshops.aws/workshops/752fd04a-f7c3-49a0-a9a0-c9b5ed40061b/en-US):
- Lab 1: AWS CodeCommit
- Lab 2: AWS CodeArtifact
- Lab 3: AWS CodeBuild
- Lab 4: AWS CodeDeploy
- Lab 5: AWS CodePipeline

## Task solution

During this workshop you will experience the AWS Code services hands-on, including:
- AWS CodeCommit as a Git repository
- AWS CodeArtifact as a managed artifact repository
- AWS CodeBuild as a way to run tests and produce software packages
- AWS CodeDeploy as a software deployment service
- AWS CodePipeline to create an automated CI/CD pipeline

You will experience the process of creating a CI/CD pipeline for a Java application deployed onto an EC2 Linux instance. Later you will containerize the application and publish to Amazon ECR before finally looking at the SAM CLI for Serverless CI/CD.

As a bonus, you will get hands-on experience using AWS Cloud9, a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser.

This workshop will take approx 3 hours to complete and labs should be completed in order. It is designed to be run at either an AWS event or in your own AWS account. No pre-created resources are required. Remember to run through the cleanup section once you have completed the workshop to avoid any undesired costs.

### Environment setup

Created Cloud9 environment "UnicornIDE".

![1](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/a77f43fd-3cde-4b08-a252-0a04062fb74b)

![2](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/f4aa8e4c-e0ea-441c-8dd9-dd1fcdb6b61b)

### Create a web app

```bash
# install Maven
sudo wget https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
sudo yum install -y apache-maven

# install Java
sudo amazon-linux-extras enable corretto8
sudo yum install -y java-1.8.0-amazon-corretto-devel
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
export PATH=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64/jre/bin/:$PATH

# verify
java -version
mvn -v
```

![3](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/97f304c8-02ae-4e8e-a041-0617c40d4b4c)

```bash
# use mvn to generate a sample Java web app
mvn archetype:generate \
    -DgroupId=com.wildrydes.app \
    -DartifactId=unicorn-web-project \
    -DarchetypeArtifactId=maven-archetype-webapp \
    -DinteractiveMode=false
```

![4](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/2ae3a633-c2e0-4b97-b19c-f209f1cbc733)

![5](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/b593784d-3116-4a31-9874-5352ad8ded18)

Modify the index.jsp file to customize the HTML code:
```jsp
<html>
<body>
<h2>Hello Unicorn World!</h2>
<p>This is my first version of the Wild Rydes application!</p>
<p>Created by adin.pilavdzija</p>
<p>DevOps Mentorship Program - AWS Community Bosnia</p>
</body>
</html>
```

### Lab 1: AWS CodeCommit

AWS CodeCommit is a secure, highly scalable, managed source control service that hosts private Git repositories. CodeCommit eliminates the need for you to manage your own source control system or about scaling its infrastructure.

In this lab we will setup a CodeCommit repository to store our Java code!

![6](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/6c9c5c5b-8dfc-47a2-93c3-493bdb186d0e)

Create a Repository `unicorn-web-project` and clone HTTPS: https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/unicorn-web-project.

```bash
# back in the Cloud9 environment setup your Git identity
git config --global user.name "adin.pilavdzija"
git config --global user.email adin.pilavdzija@edu.fit.ba

# make sure you are in the ~/environment/unicorn-web-project and init the local repo and set the remote origin to the CodeCommit URL you copied earlier
cd ~/environment/unicorn-web-project
git init -b main
git remote add origin https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/unicorn-web-project

# now we can commit and push our code
git add *
git commit -m "Initial commit"
git push -u origin main
```

![7](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/7040b8f1-daca-4e42-b3df-c66b4688524f)

![8](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/fc9972c8-f572-41cb-aca5-945e97500f4b)

Nice work! Now we have a working CodeCommit repository to store and version control our code! Next, we need a way to fetch the packages to produce our Java WAR file. Introducing AWS CodeArtifact!

### Lab 2: AWS CodeArtifact

AWS CodeArtifact is a fully managed artifact repository service that makes it easy for organizations of any size to securely fetch, store, publish, and share software packages used in their software development process.

In this lab we will setup a CodeArtifact repository that we will be using during the build phase with CodeBuild to fetch Maven packages from a public package repository (the "Maven Central Repository"). Using CodeArtifact rather than the public repository directly has several advantages, including improved security, as you can strictly define which packages can be used. To see other advantages of using CodeArtifact, please refer to the AWS CodeArtifact features web page.

Within this workshop, we will use CodeArtifact as a simple package cache. This way, even if the public package repository would become unavailable, we could still build our application. In real-world scenarios this can be an important requirement to mitigate the risk that an outage of the public repository can break the complete CI/CD pipeline. Furthermore, it helps to ensure that packages, which your project depends on, and which are (accidentally, or on purpose) being removed from the public package repository, don't break the CI/CD pipeline (as they are still available via CodeArtifact in that case).

![9](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/5465cd6e-abb5-4b71-aea9-d468bfda99c2)

In CodeArtifact service, create domain "unicorns". Then create repository "unicorn-packages" and give it a description. Select maven-central-store as public upstream repository and click Next.

![10](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/938121ae-74dd-4fda-9a0f-4a7bd20e3b8d)

#### Connect the CodeArtifact repository

- On the next page, click View connection instructions. In the dialog, choose `Mac & Linux` for Operating system and `mvn` as package manager.
- Copy the command for the authorization token and run it in your Cloud9 command prompt. This will look similar to the below. Be sure to adjust the `domain-owner` and, if present, the `region` to your account ID and region, respectively.

```bash
export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain unicorns --domain-owner 462469983678 --region eu-central-1 --query authorizationToken --output text`
```

- For the next steps, we'll have to update the settings.xml. As this doesn't exist yet, let's create it first:
```bash
cd ~/environment/unicorn-web-project
echo $'<settings>\n</settings>' > settings.xml 
```

- Open the newly created `settings.xml` in the Cloud9 directory tree and follow the remaining steps in the `Connection instructions` dialog in the CodeArtifact console including the mirror section. The complete file will look similar to the one below. Close the dialog when finished by clicking Done.

![11](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/c599f2e8-890e-4fac-81e4-9405a6ba0960)

![12](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/7d31afc7-cbb9-4e49-a532-f0f66bf9dac8)

- Let's define an IAM policy `codeartifact-unicorn-consumer-policy` so that other services can consume our newly created CodeArtifact repository. In the AWS Console, search for IAM, select it, and click Policies in the menu on the left. Click Create policy and select the JSON tab on top to view the raw JSON code of the IAM policy, then copy/paste the policy code below which will make sure that other services such as CodeBuild will be able to read the packages in our CodeArtifact repository: 

```json
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Effect": "Allow",
          "Action": [ "codeartifact:GetAuthorizationToken",
                      "codeartifact:GetRepositoryEndpoint",
                      "codeartifact:ReadFromRepository"
                      ],
          "Resource": "*"
      },
      {       
          "Effect": "Allow",
          "Action": "sts:GetServiceBearerToken",
          "Resource": "*",
          "Condition": {
              "StringEquals": {
                  "sts:AWSServiceName": "codeartifact.amazonaws.com"
              }
          }
      }
  ]
}
```

![13](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/7236e29c-8940-4d5e-a004-39fe1796c23e)

### Lab 3: AWS CodeBuild

AWS CodeBuild is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy. You can get started quickly with prepackaged build environments, or you can create custom build environments that use your own build tools.

In this lab we will setup a CodeBuild project to package our application code into a Java Web Application Archive (WAR) file.

![14](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/4f8d4921-f6dc-4019-9bc3-d1c49906bfc6)

- Create an S3 bucket named "unicorn-build-artifacts-10" with default options to store the output from CodeBuild i.e. our WAR file.
- In CodeBuild service, create build project "unicorn-web-build" with a description and a tag with `key: team` and `value: devops`. Under source select AWS CodeCommit as the source provider and select the unicorn-web-project as the repository. The branch should be main with no Commit ID. Choose other options as specified.

![15](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/5255eddc-f4a2-4cf9-8312-e6b7a3ef7b28)

Create the buildspec.yml file in the root of the code repository with specified code and your own domain-owner account ID.

![16](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/ccf4972b-446a-4ad0-a6c2-9eacc7ccd661)

Modifying the IAM role: attach policy `codeartifact-unicorn-consumer-policy` to `codebuild-unicorn-web-build-service-role`.

To test the build project, select the unicorn-web-build project in CodeBuild and select Start build > Start now.

![17](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/26aea372-8bb5-4176-9a40-493aa7b40e99)

![18](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/d3163833-ca08-4b76-ae8b-544a51c9dd52)

### Lab 4: AWS CodeDeploy

AWS CodeDeploy is a fully managed deployment service that automates software deployments to a variety of compute services such as Amazon EC2, AWS Fargate, AWS Lambda, and even on-premise services. You can use AWS CodeDeploy to automate software deployments, eliminating the need for error-prone manual operations.

In this lab, we will use CodeDeploy to install our Java WAR package onto an Amazon EC2 instance running Apache Tomcat.

![19](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/8b4ccb7e-5b52-4e5b-9b66-058d8fef37cd)

Created AWS CloudFormation Stack "UnicornStack" with provided YAML Template to provision a VPC and an EC2 instance to deploy our application to.

![20](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/7baaf8d3-83d7-43aa-9319-a714dc18c4dc)

![21](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/7312bc2b-6620-4a1f-b19a-f2193631e4ea)

Created folder `scripts` with `install_dependencies.sh`, `start_server.sh` and `stop_server.sh`. Then `appspec.yml` and modified `buildspec.yml` to ensure that that the newly added scripts folder and appspec.yml file are available to CodeDeploy. Commited all the changes to CodeCommit and in CodeBuild started build to run the `unicorn-web-build` project again which will include our newly added artifacts in the zip package.

![22](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/497319c0-06a3-4ad7-b75c-902b28495a4c)

![23](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/e9c2c32e-ec2b-4a99-97d5-fa9097322145)

CodeDeploy requires a service role to grant it permissions to the desired compute platform. For EC2/On-Premises deployments you can use the AWS Managed AWSCodeDeployRole policy. Created role `UnicornCodeDeployRole` for CodeDeploy as the service and then select CodeDeploy for the use case with the `AWSCodeDeployRole default policy`.

In CodeDeploy, created application `unicorn-web-deploy` with EC2/On-premises as the Compute platform.

![24](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/6b2b3c6c-5165-402e-8801-e51325e45a9c)

Under the `unicorn-web-deploy` application, created deployment group `unicorn-web-deploy-group` with specified options.

![25](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/e3f2b360-a40a-4c16-8f83-3294c4882f07)

In the `unicorn-web-deploy-group` created deployment as specified.

![26](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/bd416379-7200-4528-ba04-b1126acfffd7)

Check that the web application is working by browsing to http://<public-ip-address> (it needs to be http:// and not https://).

![27](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/b31318a4-cd9d-4333-9b87-5c9b002e6b65)

### Lab 5: AWS CodePipeline

AWS CodePipeline is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates. You only pay for what you use.

In this lab we use CodePipeline to create an automated pipeline using the CodeCommit, CodeBuild and CodeDeploy components created earlier. The pipeline will be triggered when a new commit is pushed to the main branch of our Git repo.

![28](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/1d4ef72b-d246-4a26-b9b4-d12fa9cb178b)

In CodePipeline service, created pipeline `unicorn-web-pipeline` as specified.

![29](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/d7cc9676-fa67-4eed-8129-afc6ba194386)

In Cloud9 environment, update the `index.jsp` as specified.

![30](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/8c288d74-364e-44b6-97cf-bca81dfca82a)

![31](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/b2797b45-2f6f-4683-aac0-79bcad1a7198)

![32](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/8d346b50-e632-4a75-bd6c-fa494740da59)

We now have a working CI/CD pipeline using the AWS Code services.

### Cleanup

- Delete CloudFormation stack: Log into the AWS Console and browse to the CloudFormation service. Select the UnicornStack and click the Delete button followed by Delete stack. This will delete the VPC, subnets and EC2 instances.
- Delete the Cloud9 environment: Log into the AWS Console and browse to the Cloud9 service. Click the UnicornIDE and click the Delete button and confirm the deletion.
- Delete CodePipeline: Log into the AWS Console and browse to the CodePipeline service. Select unicorn-web-pipeline, click the Delete pipeline button and confirm the deletion.
- Delete the CodeCommit repository: Log into the AWS Console and browse to the CodeCommit service. Select repositories, click the unicorn-web-project, click the Delete repository button and confirm the deletion.
- Delete the CodeArtifact repository and domain: Log into the AWS Console and browse to the CodeArtifact service. Select Repositories, select maven-central-store, click the Delete button and confirm the deletion. Repeat the last step for the unicorn-packages repository. Select Domains, select unicorn, click the Delete button and confirm the deletion.
- Delete CodeBuild projects: Log into the AWS console and browse to the CodeBuild service. Select Build projects and delete the unicorn-web-build project.
- Delete CodeDeploy application: Log into the AWS console and browse to the CodeDeploy service. Select Applications and view details on the unicorn-web-deploy application. Click the Delete application button and confirm the deletion.
- Delete artifact S3 bucket: Log into the AWS Console and browse to the S3 service. Select your unicorn-build-artifacts bucket, click Empty and confirm the dialog. Select the bucket again, click Delete and confirm once more.
- Empty bucket created by CodePipeline: Still in the S3 console, search for a bucket with a name similar to codepipeline-<region>-<accountid/>. Select this bucket, click Empty and confirm. You do not need to delete this bucket.
- Delete IAM Policies: Log into the AWS Console and browse to the IAM service. Select Policies and then filter for unicorn. Select codeartifact-unicorn-consumer-policy, click Actions > Delete and confirm the deletion. Do the same for all policies that have unicorn-web in their name.
- Delete IAM Roles: Still in the IAM console, select Roles in the menu. Filter for unicorn. Select UnicornCodeDeployRole, click Delete and confirm the deletion. Do the same for all policies that have unicorn-web in their name.
- Delete CloudWatch Logs group: Log into the AWS Console and browse to the CloudWatch Service. Select Log groups from the left-hand link panel. Select the unicorn-build-logs group, select Actions > Delete log group(s) and confirm the deletion.