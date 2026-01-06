# MIGRATION TO TERRAFORM & DRIFT DETECTION

https://youtu.be/-4IMy5ihiiU


Commands for Migration(importing resource files for the already created manual resources):
In the main.tf, add the below code  & then run the following command

import{
   id = "instance-id from aws"
   to = aws_instance.example

}

-> Terraorm init
-> Terraform import -generate-config-out=generated-resource.tf
A file will be created with the details of the reource provided in the import

Copy the complete resource from generated-resource.tf & paste in main.tf instead of import & run the below command which will create a statefile for the resource.

-> terraform import aws_instance.example "instance-id"


Commands to Detect Drift:
1. terraform refresh -> allows us to refresh the state file(problem is we will need to set up a cron job on intervals)(refresh is not fully endorsed by terraorm itself, a deprecation is going on for this)
2. If in AWS, configure very strict IAM rules, even as a devops engineer, you are not allowed to login to AWS(like view access only or approvals for any change)
3. We can setup some audit logs, we can setup up some automation in this, to identify if someone makes a manual change to the resources that are configured by terraform, and if the resource is managed by terraform by using lambda function, in the lambda function we can provide the list of resources managed by terraform. If a manual change is made to the terraorm managed resource, this labda function will go to the audit log, identify that this is an unwanted change & immediately send out notifiactions to the team members.
