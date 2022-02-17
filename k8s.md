# K8S - COMMANDS

## Create Cluster

```bash
eksctl create cluster --name=eksdemo1 \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup 
```

## Get List of clusters

```bash
eksctl get cluster   
```

## Replace with region & cluster name

```bash
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster eksdemo1 \
    --approve
```

## Create Public Node Group

```bash
eksctl create nodegroup --cluster=eksdemo1 \
                       --region=us-east-1 \
                       --name=eksdemo1-ng-public1 \
                       --node-type=t3.micro \
                       --nodes=4 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=10 \
                       --ssh-access \
                       --ssh-public-key=ec2_ssh1 \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access 
```

## Verify NodeGroup subnets to confirm EC2 Instances are in Public Subnet

Verify the node group subnet to ensure it created in public subnets

+ Go to Services -> EKS -> eksdemo -> eksdemo1-ng1-public
+ Click on Associated subnet in Details tab
+ Click on Route Table Tab.
+ We should see that internet route via Internet Gateway (0.0.0.0/0 -> igw-xxxxxxxx)
  
Verify Cluster, NodeGroup in EKS Management Console

+ Go to Services -> Elastic Kubernetes Service -> eksdemo1

## Capture Node Group name

```bash
eksctl get nodegroup --cluster=eksdemo1
```

## Delete Node Group

```bash
eksctl delete nodegroup --cluster=eksdemo1 --name=eksdemo1-ng-public1
```

## Delete Cluster

```bash
eksctl delete cluster eksdemo1
```

## Our kubectl context should be automatically changed to new cluster

```bash
kubectl config view --minify
```

## Get Worker Node Status with wide option

```bash
kubectl get nodes -o wide
```

## Replace Pod Name, Container Image

```bash
kubectl run my-first-pod --image stacksimplify/kubenginx:1.0.0
kubectl run flask-pod --image=akhilmovva/flask-image --env="ENV=dev" --env="MYSQL_HOST=mysql.dev.akhilmovva.live" --port=5000
kubectl run mysql-pod --image=mysql:8 --env="MYSQL_ROOT_PASSWORD=akhil123" --port=3306
```

## Alias name for pods is po

```bash
kubectl get po
kubectl get pods -o wide
```

## Describe the Pod

```bash
kubectl describe pod my-first-pod 
```

## Delete Pod

```bash
kubectl delete pod my-first-pod
```

## Expose Pod as a Service

```bash
kubectl expose pod my-first-pod  --type=NodePort --port=80 --name=my-first-service
kubectl expose pod flask-pod --type=NodePort --port=5000 --target-port=5000 --name=flask-service
kubectl expose pod mysql-pod --type=NodePort --port=3306 --target-port=3306 --name=mysql-service
```

## Get Service Info

```bash
kubectl get service
kubectl get svc
```

## Dump Pod logs

```bash
kubectl logs my-first-pod
```

## Stream pod logs with -f option and access application to see logs

```bash
kubectl logs -f my-first-pod
```

## Connect to Nginx Container in a POD

```bash
kubectl exec -it my-first-pod -- /bin/bash
kubectl exec -it mysql-pod -- bash
kubectl exec <pod-name> -- nslookup nginx
kubectl exec <pod-name> -- wget -q0- http://nginx
```

## Sample Commands

```bash
kubectl exec -it my-first-pod env
kubectl exec -it my-first-pod ls
kubectl exec -it my-first-pod cat /usr/share/nginx/html/index.html
```

## Get pod definition YAML output

```bash
kubectl get pod my-first-pod -o yaml
```

## Get service definition YAML output

```bash
kubectl get service my-first-service -o yaml
```

## Get all Objects in default namespace

```bash
kubectl get all
```

## Delete Services

```bash
kubectl delete svc my-first-service
kubectl delete svc my-first-service2
kubectl delete svc my-first-service3
```

## Useful commands

```bash
kubectl rollout restart deployment nfs-client-provisioner -n storage
```

## TIPS

```text
kubernetes.io/cluster/eksdemo1 = shared
```

```text
arn:aws:iam::705413410358:policy/ALBIngressControllerIAMPolicy
```

## Replaced region, name, cluster and policy arn (Policy arn we took note in step-03)

```text
eksctl create iamserviceaccount \
    --region us-east-1 \
    --name alb-ingress-controller \
    --namespace kube-system \
    --cluster eksdemo1 \
    --attach-policy-arn arn:aws:iam::705413410358:policy/ALBIngressControllerIAMPolicy \
    --override-existing-serviceaccounts \
    --approve
```

## Edit Deployment

```bash
kubectl edit deployment.apps/alb-ingress-controller -n kube-system
```

## Replaced cluster-name with our cluster-name eksdemo1

```text
    spec:
        containers:
        - args:
        - --ingress-class=alb
        - --cluster-name=eksdemo1
```

```text
arn:aws:iam::705413410358:policy/AllowExternalDNSUpdates
```

## Replaced name, namespace, cluster, arn

```text
eksctl create iamserviceaccount \
    --name external-dns \
    --namespace default \
    --cluster eksdemo1 \
    --attach-policy-arn arn:aws:iam::705413410358:policy/AllowExternalDNSUpdates \
    --approve \
    --override-existing-serviceaccounts
```

## IMPORTANT LINKS

### Nginx Ingress

+ Ingress installation steps <https://kubernetes.github.io/ingress-nginx/deploy/#aws>

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml
```

#### Installation - <https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/01-EKS-Create-Cluster-using-eksctl/01-02-Create-EKSCluster-and-NodeGroups>
