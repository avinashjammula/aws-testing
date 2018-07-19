# aws-testing
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
Title: AWS-SOP-101537 AWS CODE Management SOP Page 1 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
AWS-SOP-101537
AWS CODE Management SOP
Version 1.0
Effective Date: 15th March 2018
Purpose: The purpose of this document is to describe the use of GitHub code repositories and
S3 buckets for the management of Lilly’s AWS Foundation.
Scope: GitHub, AWS S3, CloudFormation and their supporting teams that collect, store or
transfer the infrastructure code used to deploy and support resource in Lilly’s AWS
Foundation.
Areas Involved: Cloud and Hosting Services and IT partners looking to consume Amazon Web
Services via Lilly’s Foundation.
Supersedes/Replaces: N/A – New Document
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 2 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
TABLE OF CONTENTS
1. INTRODUCTION.................................................................................................................................................3
1.1 GENERAL RULES ............................................................................................................................................3
1.2 REFERENCES .................................................................................................................................................3
1.3 TERMS/ACRONYMS.........................................................................................................................................3
2. CODE MANAGEMENT PROCESS ....................................................................................................................4
3. CODE MANAGEMENT IN GITHUB ...................................................................................................................5
3.1 GITHUB REPOSITORY SETUP ..........................................................................................................................5
3.1.1 Managing Admins: ............................................................................................................................... 5
3.2 GITHUB ISSUES..............................................................................................................................................6
3.2.1 To create an issue: .............................................................................................................................. 6
3.3 PULL REQUESTS ............................................................................................................................................6
3.3.1 Creating a pull request......................................................................................................................... 6
4. DEPLOYING CHANGES ....................................................................................................................................7
4.1 CLOUDFORMATION STACK DEPLOYMENT.........................................................................................................7
4.1.1 To update the stack from the AWS Management Console ................................................................. 7
5. TRAINING IN THIS PROCEDURE .....................................................................................................................8
REVISION HISTORY: ................................................................................................................................................9
AUTHOR AND REVIEWERS...................................................................................................................................10
PROCEDURE APPROVAL SIGNATURES:............................................................................................................10
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 3 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
1. INTRODUCTION
Amazon Web Services (AWS) is a secure, scalable cloud services platform, offering compute power, database
storage, content delivery and other functionality to help businesses scale and grow. It provides a mix of
infrastructure as a service (IaaS), platform as a service (PaaS) and packaged software as a service (SaaS)
offerings.
The intent of Lilly’s AWS Foundation services implementation is to provide a secure, scalable Virtual Data Center
eco-system along with services to enable the basic platform for application and service owners to host their
applications and services in the public cloud. AWS provides management APIs that control functionality of all
operations on the platform. Managing systems at scale requires automation in order to remove error prone
manual operations. AWS provides an easy service for applying this principle through the usage of AWS Cloud
Formation templates. Using these templates allows for the deployment of infrastructure as code quickly and
efficiently in a controlled and secure manner. This document describes the process used for managing this code
through the use of GitHub, S3 and CloudFormation.
1.1 General Rules
All changes made to Lilly’s foundation that is laid out in AWS-RAD-101527 AWS Foundational Design will be
performed under an approved Change Request
1.2 References
The following are referred to in this procedure:
AWS-RAD-101527 AWS Foundational Design
ITC-SOP-CHANGE MANAGEMENT-412 IT COMMON SOP – CHANGE MANAGEMENT
1.3 Terms/Acronyms
Term Definition
AWS Amazon Web Services
CloudFormation AWS service for managing and deploying infrastructure as code
CLI Command Line Interface
GitHub Web-based version control service for git
VPC Virtual Private Cloud
YAML Yet Another Markup Language
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 4 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
2. CODE MANAGEMENT PROCESS
This section is an overview of the process used to manage the code used to create Lilly’s Foundation within the
AWS cloud. This section will define the high-level process and then refer to further sections within this document
to get more detailed information.
• A change to the existing foundational services is identified, this could be a bug / enhancement request /
new service. A Change Request within ServiceNow will be raised to track the change, gain approvals
and document the deployment. This Change Request will follow the standard Change Process.
• If the Change request is approved for development, the code will be updated following Section 3 Code
Management in GitHub
• Once the development has been performed, and the Code has been merged back into the master
branch, the Change Request needs to be approved for deployment.
• The Code can then be copied to the S3 Bucket and deployed, See Section 4.
• Once the code has successfully been deployed, the Change Request can then be closed.
At closure the Change Request should contain
• Reason for the Change
• Output from the Testing in Sandbox environment
• Link to the Pull Request
• Evidence of successfully deployment into the Production VPCs
• Approvals for development and deployment as per the ITC SOP – Change Management depending on
change type.
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 5 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
3. CODE MANAGEMENT IN GITHUB
The following sections describe the use of GitHub to store CloudFormation template code, manage access and
privileges and also the change management and deployment process using CloudFormation and S3 from within
the associated AWS VPC. The diagram below shows an overview of the process and takes into account the
remote S3 storage location and CloudFormation usage of the code. Commonly used text editors are displayed as
an example.
3.1 GitHub Repository Setup
Github Repositories that contain AWS VPC code are named GIS_IIHS_AWS_(vpcName). The repositories
contain a branch hierarchy that has been created to enable proper code management. The
The master branch contains the current live code that was used to deploy the VPC. To deploy the VPC the
templates stored on the branch are first copied to an S3 bucket which CloudFormation has access to. See
Section 4 for more details on how templates are deployed.
The develop branch contains news features that are ready to be merged into master. This code will be tested in a
sandbox environment and is ready to be merged to master and stored in s3 before deployment via
CloudFormation.
Feature branch(es) contain code that is not yet ready for testing and is currently in development. There can be
many feature branches at any given time. Feature branches are direct clones of the current master branch and
can be created and deleted as needed. When a new feature matures, it is merged into the develop branch. The
development branch equates to a collection of new features that make up a new release into master.

To enable the change flow of code, administrators of the repository (at least 2 per repo as a minimum) have been
chosen and are responsible for managing what code moves from newly created feature branches into
development and finally through to master. This process is managed through the use of GitHub Issues and Pull
Requests.
3.1.1 Managing Admins:
1. Ask for the username of the person you're inviting as a collaborator.
2. On GiHub, navigate to the main page of the repository
3. Under your repository name, click Settings.
4. In the left sidebar, click Collaborators.
5. Under Collaborators, start typing the collaborator’s username
6. Select the collaborator's username from the drop-down menu
7. Click Add collaborator.
8. The user will receive an email inviting them to the repository. Once they accept your invitation, they will
have collaborator access to your repository.
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 6 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
3.2 GitHub Issues
GitHub issues are used to log either new features that should / could be worked on or problems / bugs with the
current code on a given branch. The focus with issues is on collaboration and they provide:
• A title and description to describe what the issue is.
• Color-coded labels help you categorize and filter your issues (just like labels in email).
• A milestone acts like a container for issues. This is useful for associating issues with specific features or
project phases (e.g. Weekly Sprint 9/5-9/16 or Shipping 1.0).
• One assignee is responsible for working on the issue at any given time.
• Comments allow anyone with access to the repository to provide feedback.
Once an issue is marked as resolved, a pull request can be submitted to let the repository admins know a new
feature is ready to be merged into the develop branch or a new release is ready to go into the master branch. To
properly ensure issues and any subsequent code changes are tracked properly from an enterprise point of view, a
change request should be raised and then used as part of the issue title.
3.2.1 To create an issue:
1. On GitHub, navigate to the main page of the repository.
2. Under your repository name, click Issues.
3. Click New issue
4. Type a title and description for your issue.
5. If you're an administrator, you can assign the issue to someone, add it to a project board, associate it with
a milestone, or apply a label.
6. When you're finished, click Submit new issue
An Issue should be raised to track the changes documented in the Change Request.
3.3 Pull Requests
Pull requests let developers tell other members of the repository about changes that they’ve pushed to a branch.
Once a pull request is sent, administrators of the repository can review the set of changes, discuss potential
modifications, and push follow-up commits if necessary.
Pull requests are opened after issues are resolved and are used by developers and administrators to have their
new code moved from branch to branch. To successfully merge a pull request, two approvals from the repository
administrator team are required, before the repository administrators approve the pull request they are required to
perform a code review to ensure that in their opinion the change will work and only the changes requested in the
Change request will be merged into the master branch
A Pull Request will not be accepted to develop without a relating Issue and CR that both reflect corresponding
criteria that clearly identifies the nature of the code change. The output of the tested code must be provided and
logged to the respective CR before moving to the master branch as well as URL to the Pull request. Moving code
to the master branch via a pull request is explained in section 3.3.1, and replicating code to the remote
deployment and storage locations, (CloudFormation, S3) are explained in section 4 of this document.
3.3.1 Creating a pull request
1. On GitHub, navigate to the main page of the repository.
2. the "Branch" menu, choose the branch that contains your commits.
3. To the right of the Branch menu, click New pull request.
4. Use the base branch dropdown menu to select the branch you'd like to merge your changes into, then
use the compare branch drop-down menu to choose the topic branch you made your changes in.
5. Type a title and description for your pull request.
6. Click Create pull request.
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 7 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
4. DEPLOYING CHANGES
Once changes have been merged to the master branch, it is necessary to deploy the new CloudFormation
templates to update the corresponding Amazon VPC. To do this, the templates that have been modified need to
be copied into the S3 bucket that Cloud Formation will use. To properly ensure that the files stored in GitHub are
exactly replicated in S3 and then finally in CloudFormation, it is necessary to make use of AWS command line
tools. Using the command line tools as opposed to the GUI provides a uniform deployment method of
deployment with exact steps that can be tracked and altered based on new AWS CLI releases.
Examples
• Copying one local file to S3
Command: aws s3 cp test.txt s3://mybucket/test2.txt
Output: upload: test.txt to s3://mybucket/test2.txt
• Copying an entire local directory to S3
The following sync command syncs objects under a specified prefix and bucket to files in a local directory by
uploading the local files to s3. A local file will require uploading if the size of the local file is different than the size
of the s3 object, the last modified time of the local file is newer than the last modified time of the s3 object, or the
local file does not exist under the specified bucket and prefix. In this example, the user syncs the bucket
mybucket to the local current directory. The local current directory contains the files test.txt and test2.txt. The
bucket mybucket contains no objects:
Command: aws s3 sync . s3://mybucket
Output: upload: test.txt to s3://mybucket/test.txt
upload: test2.txt to s3://mybucket/test2.txt
4.1 CloudFormation Stack Deployment
Once the CloudFormation templates are copied from the GitHub repository master branch into S3 they can be
deployed using the AWS CloudFormation Console. To update a stack:
4.1.1 To update the stack from the AWS Management Console
1. Log in to the AWS CloudFormation console, at: https://console.aws.amazon.com/cloudformation.
2. On the AWS CloudFormation dashboard, select the previously deployed top-level stack, and then click
Update Stack.
3. In the Update Stack wizard, on the Select Template screen, select Upload a template to Amazon S3,
select the modified template, and then click Next.
4. On the Options screen, click Next.
5. Click Next because the stack doesn't have a stack policy. All resources can be updated without an
overriding policy.
6. On the Review screen, verify that all the settings are as you want them, and then click Update. The input
parameters will be imported from previous deployment.
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 8 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
The output of a successful stack update can be retrieved from AWS console logs and must be logged against the
raised CR. The CR can then be closed providing that the code making up the release has been merged to the
master branch via the Pull Request and Issue process.
5. TRAINING IN THIS PROCEDURE
All personnel using this process must train on this procedure by reading the document and recording training in
the corporate learning management system.
If you are using a printed copy of this document, please check that it is consistent with the current, official version.
ELI LILLY AND COMPANY
AWS CODE Management SOP
Title: AWS-SOP-101537 AWS CODE Management SOP Page 9 of 10 Version 1.0
Owner: Tom Griffin. Normal Business Last Save Date: 06-Mar-2018
REVISION HISTORY:
Summary of Changes for Current Version
Significant changes include:
• New Document
Version Effective Date Author/Reviser
1.0 15th March 2018 Tom Griffin

