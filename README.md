# EKS Made Easy
https://www.youtube.com/playlist?list=PL8klaCXyIuQ65hRm3pPIRg2OSRjOsXWq2

## Prerequisites

### Installing AWS CLI
- https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html
- https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html
- https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-instances.html
- https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html

```
aws --version
aws configure help
```

### Installing kubectl
- https://kubernetes.io/docs/tasks/tools/


### Installing eksctl
- https://aws.amazon.com/eks/
- https://docs.aws.amazon.com/eks/
- https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
- https://eksctl.io/
- https://github.com/weaveworks/eksctl


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

```
# Syntax
eksctl create cluster --name cluster-name  \
--region region-name \
--version 1.23 \
--nodegroup-name test \
--node-type instance-type \
--node-volume-size <GB> \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ1>,<AZ2>

# Example
eksctl create cluster --name demo --region=ap-south-1 --nodegroup-name demo --nodes 2 --nodes-min 1 --nodes-max 4 --node-volume-size 8 

# Get list of clusters
eksctl get cluster --region ap-south-1

# Delete EKS Cluster through EKSCTL
eksctl delete cluster --name demo --region=ap-south-1
```

## Create Control Plane and Node Group seperately

```
# Create Cluster
eksctl create cluster --name=demo --region=ap-south-1 --without-nodegroup 

# Get list of clusters
eksctl get cluster    

# Delete the cluster
eksctl delete cluster demo --region=ap-south-1
```

### Create Node Group with IAM policies to access ALB, DNS, ECR, ASG etc. You should first create a keypair(RSA, .pem) to be used for SSH access to the worker nodes
```
# Help
eksctl create nodegroup --help

# Create NodeGroup with IAM policies to access ALB, DNS, ECR, ASG
eksctl create nodegroup --name=demo \
                       --managed \
                       --cluster=demo \
                       --tags "env=test,type=demo" \
                       --region=ap-south-1 \
                       --node-type=t2.micro \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=8 \
                       --ssh-access \
                       --ssh-public-key=ekstestpair \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --appmesh-preview-access \
                       --alb-ingress-access 

# Get NodeGroup
eksctl get nodegroup --cluster=demo

# Explore the cluster, check ASG, EKS Nodes Group, EC2, KeyPairs, IAM Policy
kubectl get nodes 
kubectl get po -A

# Delete Node Group
eksctl delete nodegroup --cluster=<clusterName> --name=<nodegroupName>
eksctl delete nodegroup --cluster=demo --name=demo
```

## Demo App
```
# Create a Pod
kubectl run nginx --image=nginx --port=80 

# Expose with LoadBalancer service
kubectl expose pod nginx --port=80 --type=LoadBalancer

# Delete the resources created
kubectl delete po/nginx svc/nginx
```
