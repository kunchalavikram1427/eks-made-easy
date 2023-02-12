# EKS Made Easy
https://www.youtube.com/playlist?list=PL8klaCXyIuQ65hRm3pPIRg2OSRjOsXWq2

## Prerequisites

### Installing AWS CLI
https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html
https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-instances.html
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html

```
aws --version
aws configure help
```

### Installing kubectl
https://kubernetes.io/docs/tasks/tools/


### Installing eksctl
https://aws.amazon.com/eks/
https://docs.aws.amazon.com/eks/
https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
https://eksctl.io/
https://github.com/weaveworks/eksctl


## EKS Cluster through Management Console
### Steps
- Create another IAM user with console and programmatic access
- Configure AWS CLI

```
aws iam list-users
```
### Get config
```
aws eks update-kubeconfig \
    --region REGION \
    --name CLUSTER_NAME 
```

## Create EKS Cluster through EKSCTL
Syntax
```
eksctl create cluster --name cluster-name  \
--region region-name \
--version 1.23 \
--nodegroup-name test \
--node-type instance-type \
--node-volume-size <GB> \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ1>,<AZ2>
```
Example
```
eksctl create cluster --name demo --region=ap-south-1 --nodegroup-name demo --nodes 2 --nodes-min 1 --nodes-max 4 --node-volume-size 8 
```

Get Cluster details
```
eksctl get cluster --region ap-south-1
```

## Delete EKS Cluster through EKSCTL
```
eksctl delete cluster --name demo --region=ap-south-1
```

