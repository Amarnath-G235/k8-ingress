# Ingress Controller

```
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster expense-1 \
    --approve
```

```
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.13.3/docs/install/iam_policy.json

```

```
Aws IAM create policy/ AWSLoadBalancerControllerIAMPolicy

aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json
```

```
Create an IAM role and Kubernetes ServiceAccount for the LBC. Use the ARN from the previous step.

eksctl create iamserviceaccount \
--cluster=expense-1 \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::211125364218:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve
```

```
Add the EKS chart repo to Helm

helm repo add eks https://aws.github.io/eks-charts

```

```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=expense-1 --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
```


