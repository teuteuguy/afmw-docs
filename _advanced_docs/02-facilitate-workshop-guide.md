---
title: Facilitate your own workshop
layout: single
sidebar:
- nav: docs
permalink: /docs/facilitate-workshop-guide
excerpt: How to setup the workshop in your account for multiple students to run the labs.
last_modified_at: '2019-11-14 09:53:43 +0800'
toc: true
toc_label: Contents
---

The following guide will help you be a workshop facilitator for multiple students. The idea here is to run a Cloudformation script in your account that will create IAM Users (Students) with extremely limited access to AWS resources to be able to run the labs.

**Warning:** The following guide is to be used knowing that having multiple students run the labs in your account, will consume AWS resources and thus costs.
{: .notice--danger}


# Pre-Requisites

Obviously, running in your account will consume AWS resources, and your account limits (for EC2) may need to be raised for your students to be able to run the workshop.

Also, the labs require having access to M5StickC devices. You can order them from M5Stack, here: [https://m5stack.com/collections/m5-core/products/stick-c](https://m5stack.com/collections/m5-core/products/stick-c).

# CloudFormation Script

The following CloudFormation script can setup your students for you.

You have to provide a *Student name prefix*, a *Password prefix*, a *Nb of Students* and a *Group name*.

For example, configuring the cloudformation deployment with the followin parameters:
- StudentPrefix: **Student**
- NbOfStudents: **3**
- PasswordPrefix: **pwd**
- GroupName: **Students**

will create the following IAM Users and their respective passwords. As well as add them to the **Students** IAM group:

| Username | Password |
| :------------ | :------------------------------------- |
| Student100 | pwdStudent100 |
| Student101 | pwdStudent101 |
| Student102 | pwdStudent102 |

## Deploy the Cloudformation script

| Region | Launch Template |
| ------------ | ------------- |
| **N. Virginia** (us-east-1) | [![Launch the Facilitator Cloudformation in us-east-1]({{ '/assets/images/deploy-to-aws.png' | relative_url }})](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=sputnik&templateURL=https://raw.githubusercontent.com/teuteuguy/afmw-docs/master/assets/cfn/facilitator-cfn.yml) |
| **Oregon** (us-west-2) | [![Launch the Facilitator Cloudformation in us-west-2]({{ '/assets/images/deploy-to-aws.png' | relative_url }})](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=sputnik&templateURL=https://raw.githubusercontent.com/teuteuguy/afmw-docs/master/assets/cfn/facilitator-cfn.yml) |
| **Singapore** (ap-southeast-1) | [![Launch the Facilitator Cloudformation in ap-southeast-1]({{ '/assets/images/deploy-to-aws.png' | relative_url }})](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/new?stackName=sputnik&templateURL=https://raw.githubusercontent.com/teuteuguy/afmw-docs/master/assets/cfn/facilitator-cfn.yml) |

# What does the CFN template actually do?

* Creates an IAM Group 

* Creates an IAM Role for the Group with the following policy:

```bash
-
    Effect: 'Allow'
    Action:
        - 'cloud9:ValidateEnvironmentName'
        - 'cloud9:UpdateUserSettings'
        - 'cloud9:GetUserSettings'
        - 'iam:GetUser'
        - 'iam:ListUsers'
        - 'ec2:DescribeVpcs'
        - 'ec2:DescribeSubnets'
    Resource: 
        - '*'
-
    Effect: 'Allow'
    Action:
        - 'cloud9:CreateEnvironmentEC2'
    Resource: 
        - '*'
    Condition:
        'Null':
            'cloud9:OwnerArn': 'true'
        'StringEquals':
            'cloud9:EnvironmentName': '${aws:username}'
            'cloud9:InstanceType': 't2.micro'
                
-
    Effect: 'Allow'
    Action:
        - 'cloud9:GetUserPublicKey'
    Resource: 
        - '*'
    Condition:
        'Null':
            'cloud9:OwnerArn': 'true'
-
    Effect: 'Allow'
    Action:
        - 'cloud9:DescribeEnvironmentMemberships'
    Resource: 
        - '*'
    Condition:
        'Null':
            'cloud9:OwnerArn': 'true'
            'cloud9:EnvironmentId': 'true'
-
    Effect: 'Allow'
    Action:
        - 'iam:CreateServiceLinkedRole'
    Resource: 
        - '*'
    Condition:
        'StringLike':
            'iam:AWSServiceName': 'cloud9.amazonaws.com'
-
    Effect: 'Allow'
    Action:
        - 'iot:CreateThing'
    Resource:
        - !Join [':', ['arn:aws:iot:*', Ref: 'AWS::AccountId', 'thing/${aws:username}']]
-
    Effect: 'Allow'
    Action:
        - 'iot:ValidateSecurityProfileBehaviors'
        - 'iot:TestInvokeAuthorizer'
        - 'iot:DescribeThingRegistrationTask'
        - 'iot:ListV2LoggingLevels'
        - 'iot:AddThingToThingGroup'
        - 'iot:ListBillingGroups'
        - 'iot:ListThingsInThingGroup'
        - 'iot:ListPolicyVersions'
        - 'iot:ListThingRegistrationTasks'
        - 'iot:ListTagsForResource'
        - 'iot:DescribeStream'
        - 'iot:DescribeIndex'
        - 'iot:CreateCertificateFromCsr'
        - 'iot:ListViolationEvents'
        - 'iot:ListCertificatesByCA'
        - 'iot:DisableTopicRule'
        - 'iot:DeletePolicyVersion'
        - 'iot:RegisterCertificate'
        - 'iot:CreateTopicRule'
        - 'iot:GetV2LoggingOptions'
        - 'iot:Receive'
        - 'iot:ListThingGroupsForThing'
        - 'iot:DetachPrincipalPolicy'
        - 'iot:ListPolicies'
        - 'iot:DescribeCACertificate'
        - 'iot:ListThings'
        - 'iot:ListIndices'
        - 'iot:DescribeJobExecution'
        - 'iot:DescribeAuditTask'
        - 'iot:ReplaceTopicRule'
        - 'iot:ListOutgoingCertificates'
        - 'iot:DeleteCertificate'
        - 'iot:AttachThingPrincipal'
        - 'iot:GetRegistrationCode'
        - 'iot:CreatePolicy'
        - 'iot:ListTargetsForSecurityProfile'
        - 'iot:DeleteThingShadow'
        - 'iot:DeleteTopicRule'
        - 'iot:GetThingShadow'
        - 'iot:CreateKeysAndCertificate'
        - 'iot:ListThingRegistrationTaskReports'
        - 'iot:GetStatistics'
        - 'iot:Publish'
        - 'iot:DescribeScheduledAudit'
        - 'iot:GetPolicy'
        - 'iot:AttachPrincipalPolicy'
        - 'iot:AttachPolicy'
        - 'iot:DescribeThingGroup'
        - 'iot:ListThingTypes'
        - 'iot:ListStreams'
        - 'iot:ListThingPrincipals'
        - 'iot:UpdateThing'
        - 'iot:ListOTAUpdates'
        - 'iot:DetachPolicy'
        - 'iot:ListJobExecutionsForJob'
        - 'iot:SetDefaultPolicyVersion'
        - 'iot:DescribeEndpoint'
        - 'iot:GetJobDocument'
        - 'iot:Subscribe'
        - 'iot:ListScheduledAudits'
        - 'iot:DescribeSecurityProfile'
        - 'iot:GetOTAUpdate'
        - 'iot:GetPolicyVersion'
        - 'iot:GetTopicRule'
        - 'iot:CreatePolicyVersion'
        - 'iot:GetIndexingConfiguration'
        - 'iot:ListAuditFindings'
        - 'iot:SearchIndex'
        - 'iot:GetPendingJobExecutions'
        - 'iot:ListPrincipalThings'
        - 'iot:EnableTopicRule'
        - 'iot:DescribeDefaultAuthorizer'
        - 'iot:DescribeEventConfigurations'
        - 'iot:DescribeAuthorizer'
        - 'iot:ListRoleAliases'
        - 'iot:RegisterThing'
        - 'iot:DeleteThing'
        - 'iot:DescribeAccountAuditConfiguration'
        - 'iot:DescribeBillingGroup'
        - 'iot:Connect'
        - 'iot:DescribeCertificate'
        - 'iot:ListCACertificates'
        - 'iot:ListJobExecutionsForThing'
        - 'iot:ListThingGroups'
        - 'iot:ListSecurityProfilesForTarget'
        - 'iot:DescribeRoleAlias'
        - 'iot:ListPrincipalPolicies'
        - 'iot:UpdateCertificate'
        - 'iot:GetEffectivePolicies'
        - 'iot:ListTopicRules'
        - 'iot:DetachThingPrincipal'
        - 'iot:ListAuditTasks'
        - 'iot:ListSecurityProfiles'
        - 'iot:DescribeThing'
        - 'iot:ListPolicyPrincipals'
        - 'iot:GetLoggingOptions'
        - 'iot:ListAttachedPolicies'
        - 'iot:UpdateThingShadow'
        - 'iot:ListActiveViolations'
        - 'iot:ListTargetsForPolicy'
        - 'iot:DeletePolicy'
        - 'iot:ListThingsInBillingGroup'
        - 'iot:TestAuthorization'
        - 'iot:ListJobs'
        - 'iot:ListAuthorizers'
        - 'iot:DescribeJob'
        - 'iot:DescribeThingType'
        - 'iot:ListCertificates'
    Resource: '*'
```

**Note:** The above policy is quite open for IoT related actions. The objective is to restrict access over time as the labs evolve.
{: .notice--info}


* Creates a custom resource CFN lambda function.

This lambda function will then programatically create your list of IAM Student users, add them to the group and create their login profile with the default password
