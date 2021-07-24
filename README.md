# terraform-aws-eks

Deploy a full AWS EKS cluster with Terraform

## What resources are created

1. VPC
2. Internet Gateway (IGW)
3. Public and Private Subnets
4. Security Groups, Route Tables and Route Table Associations
5. IAM roles, instance profiles and policies
6. An EKS Cluster
7. EKS Managed Node group
8. Autoscaling group and Launch Configuration
9. Worker Nodes in a private Subnet
10. bastion host for ssh access to the VPC
11. The ConfigMap required to register Nodes with EKS
12. KUBECONFIG file to authenticate kubectl using the `aws eks get-token` command. needs awscli version `1.16.156 >`

## Configuration

You can configure you config with the following input variables:

## Configuration

You can configure you config with the following input variables:

```terraform
module "eks" {
  source  = "./moddules/eks"

  aws-region          = "us-west-1"
  availability-zones  = ["us-west-1a", "us-west-1b", "us-west-1c"]
  cluster-name        = "my-cluster"
  k8s-version         = "1.17"
  node-instance-type  = "t3.medium"
  root-block-size     = "40"
  desired-capacity    = "3"
  max-size            = "5"
  min-size            = "1"
  vpc-subnet-cidr     = "10.0.0.0/16"
  private-subnet-cidr = ["10.0.0.0/19", "10.0.32.0/19", "10.0.64.0/19"]
  public-subnet-cidr  = ["10.0.128.0/20", "10.0.144.0/20", "10.0.160.0/20"]
  db-subnet-cidr      = ["10.0.192.0/21", "10.0.200.0/21", "10.0.208.0/21"]
  eks-cw-logging      = ["api", "audit", "authenticator", "controllerManager", "scheduler"]
  ec2-key-public-key  = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD3F6tyPEFEzV0LX3X8BsXdMsQz1x2cEikKDEY0aIj41qgxMCP/iteneqXSIFZBp5vizPvaoIR3Um9xK7PGoW8giupGn+EPuxIA4cDM4vzOqOkiMPhz5XK0whEjkVzTo4+S0puvDZuwIsdiW9mxhJc7tgBNL0cYlWSYVkz4G/fslNfRPW5mYAM49f4fhtxPb5ok4Q2Lg9dPKVHO/Bgeu5woMc7RY0p1ej6D4CKFE6lymSDJpW0YHX/wqE9+cfEauh7xZcG0q9t2ta6F6fmX0agvpFyZo8aFbXeUBr7osSCJNgvavWbM/06niWrOvYX2xwWdhXmXSrbX8ZbabVohBK41 email@example.com"
}

output "kubeconfig" {
  value = module.eks.kubeconfig
}

output "config-map" {
  value = module.eks.config-map-aws-auth
}

```

## verify

- create kube.conf from output
- run command

```bash
kubectl --kubeconfig ./kube.conf config view
kubectl --kubeconfig ./kube.conf get nodes
kubectl --kubeconfig ./kube.conf describe node xxx
```
