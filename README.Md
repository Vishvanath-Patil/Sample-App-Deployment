## Create Cluster and node group creation
```bash
eksctl create cluster --name my-cluster  --region ap-south-1 --nodegroup-name my-nodegroup --node-type t3a.medium --nodes 2 --nodes-min 1 --nodes-max 2 --node-volume-size=20 --ssh-access --ssh-public-key eks-demo --managed --asg-access --external-dns-access --full-ecr-access --appmesh-access --alb-ingress-access
```
## Enable OIDC provider
```bash
eksctl utils associate-iam-oidc-provider --region ap-south-1 --cluster my-cluster --approve
```
## Create IAM service account
```bash
eksctl create iamserviceaccount --cluster=my-cluster --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::237924323600:policy/AWSLoadBalancerControllerIAMPolicy --region ap-south-1 --approve
```
## Install AWS Load Balancer controller
```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=my-cluster --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller --set replicaCount=1
```
AWS Official Document : https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html

## Tags to add in public subnet if you get unable to auto disover subnet error:
```bash
kubernetes.io/role/elb = 1
kubernetes.io/cluster/my-cluster = shared
```
## Clean up:
### Delete Load balancer first then
### delete all resources using below command
```bash
eksctl delete cluster --name my-cluster --region ap-south-1
```
