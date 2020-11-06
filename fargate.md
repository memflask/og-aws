+++
memflask = True
isdraft = False
+++

# Fargate

## Fargate Basics
-	📒 [Homepage](https://aws.amazon.com/fargate/) 
-   [FAQ](https://aws.amazon.com/fargate/faqs/) 
-   [Pricing](https://aws.amazon.com/fargate/pricing/)
-   **Fargate** allows you to manage and deploy containers without having to worry about running the underlying compute infrastructure
-   Fargate serves as a new backend (in addition to the legacy EC2 backend) on which ECS and EKS tasks can be run
-   Fargate and EC2 backends are called "Launch Types"
-   Fargate allows you to treat containers as fundamental building blocks of your infrastructure

## Fargate and Lambda 
-   Fargate follows a similar mindset to Lambda, which lets you focus on applications, 
instead of dealing with underlying infrastructure

## Fargate - Native support tools
-   Fargate is supported by CloudFormation, aws-cli and ecs-cli
-   Fargate tasks can be launched alongside tasks that use EC2 Launch Type

## Fargate - costing in a large deployment
-   Before creating a large Fargate deployment, make sure to estimate costs and compare them against alternative solution that uses traditional EC2 deployment - 
Fargate prices can be several times those of equivalently-sized EC2 instances. 
To evaluate both solutions based on potential costs, refer to pricing for [EC2](https://aws.amazon.com/ec2/pricing/) and [Fargate](https://aws.amazon.com/fargate/pricing/).

## Fargate Alternatives and Lock-in
-   🚪[Azure Container Instances](https://azure.microsoft.com/en-us/services/container-instances/): Available on Microsoft Azure in preview version, allows to run applications in containers without having to manage virtual machines

## Fargate Gotchas only used with ECS
-    Fargate can only be used with ECS. Support for EKS [was originally planned for 2018](https://aws.amazon.com/blogs/aws/aws-fargate/), but has yet to launch.
## Fargate smallest resources
-   The smallest resource values that can be configured for an ECS Task that uses Fargate is 0.25 vCPU and 0.5 GB of memory
## Storage in Fargate
-   [Task storage is ephemeral. After a Fargate task stops, the storage is deleted.](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-task-storage.html)
