# 05 Create AWS Account and IAM Users

## Task

In the week-6 of our classes, we started learning about AWS. For this week we have the following tasks which will be used to prepare our AWS account for future use. This task is considered done when you can mark all subtasks as completed.

Account Owners need to create AWS Accounts.  
If you see your name inside the document *DevOps Mentorship Program - AWS Account Owners* document in column **Account Owner** you need to create an AWS Account. Once when AWS Account is created do the following.

**Part 1:**

*   Create MFA for root user
*   Log out from your AWS account and log in back to verify that you can access using root user and your MFA code.
*   Give your account an alias `firstname-lastname-devops-mentorship`
*   Go into the `Account Settings` and allow **IAM User and Role Access to Billing Information**
*   Go into the `IAM Dashboard` and create `IAM User` for yourself, `IAM User` should be named `firstname.lastname`
*   IAM User should be part of the `IAM Group` called `Administrators`
*   IAM Group Administrators should have `AdministratorAccess` permissions attached
*   Inside IAM Dashboard, click on `Account settings` on the left-hand side and check `Password policy` for your account.
*   Login to your account using your IAM user instead of root user.
*   Create new IAM Group called `Tier2`. Do not attach any policies to that group.
*   Ping Dzenan or Boris via slack and ask them to send you AWS Credits, apply AWS Credits code into your AWS Account

You are now ready for part 2 of this ticket.

*   Check out the document [DevOps Mentorship Program - AWS Account Owners](https://docs.google.com/spreadsheets/d/1OChZSLmr2Bh89L-bPKWPVUUAEKbTQEKWVD1PVNLUmKM/edit?usp=sharing) and find which additional IAM Users you need to create. IAM Users need to be in the format `firstname.lastname`.
*   Add them into `Tier2` IAM Group
*   Ping new users via slack and send them `.csv` access details for their IAM Users
*   Ask them to log in to AWS Account and verify access
*   Once when your IAM Users successfully log in into the AWS Account and confirm their access you can put a note into the excel file inside column `Account Owners Note`

## Task solution

- **Create MFA for root user**
   - Navigate to the AWS Management Console.
   - Access the root user's security credentials.
   - Set up Multi-Factor Authentication (MFA) for the root user account.
- **Give your account an alias `firstname-lastname-devops-mentorship`**
   - Navigate to the AWS Management Console.
   - Access the account settings.
   - Set an account alias to `adin-pilavdzija-devops-mentorship`.
- **Go into the `Account Settings` and allow IAM User and Role Access to Billing Information**
   - Navigate to the AWS Management Console.
   - Access the account settings.
   - Enable IAM User and Role Access to Billing Information.
- **Go into the `IAM Dashboard` and create `IAM User` for yourself, `IAM User` should be named `firstname.lastname`**
   - Navigate to the IAM Dashboard.
   - Create a new IAM User named `adin.pilavdzija`.
- **IAM User should be part of the `IAM Group` called `Administrators`**
   - Access the IAM Dashboard.
   - Add the `adin.pilavdzija` IAM User to the `Administrators` IAM Group.
- **IAM Group Administrators should have `AdministratorAccess` permissions attached**
   - Access the IAM Dashboard.
   - Attach the `AdministratorAccess` permissions policy to the `Administrators` IAM Group.
- **Inside IAM Dashboard, click on `Account settings` on the left-hand side and check `Password policy` for your account.**
   - Navigate to the IAM Dashboard.
   - Access the account settings.
   - Review the password policy for the account.
- **Create a new IAM Group called `Tier2`. Do not attach any policies to that group.**
   - Navigate to the IAM Dashboard.
   - Create a new IAM Group named `Tier2` without attaching any policies.
- **Apply AWS Credits code into your AWS Account**
   - Access the billing section of the AWS Management Console.
   - Redeem and apply the AWS Credits code to your AWS account.