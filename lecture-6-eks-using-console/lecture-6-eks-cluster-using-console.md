# Create EKS K8s Cluster using AWS Management Console

In this lecture, we are going to deploy the below Architecture
![lecture_6_architecture](https://user-images.githubusercontent.com/121017040/211267425-de8123c6-a955-444f-b5ca-b6d3985a63e4.png)

## Youtube Lecture Link [Language : English-Hindi Mix]

[<img src="https://user-images.githubusercontent.com/121017040/211271491-babf1b23-2f75-4709-b005-3d8ca652985e.png" width="100%">](https://youtu.be/06_oEmEDnZg "Kubernetes Series - Lecture 6")
[EKS](https://youtu.be/06_oEmEDnZg "Kubernetes Series - Lecture 6")

### Note: Please make sure you are using the same credentials for both one to create your EKS cluster and another to communicate with your Kubernetes cluster

In the above architecture, we are deploying the below resources:

1. Two VPCs - One for Bastion Host  ||  2nd for Kubernetes Cluster - `Free-Tier Eligible`
2. One Bastion Host using Cloud9 Environment - `Free-Tier Eligible`
3. Two NAT Gateways - `Free-Tier Not Eligible`
4. One EKS Cluster - `Free-Tier Not Eligible`
5. Three AWS Managed Nodes - `Free-Tier Not Eligible`

## Implementation Guide:

### Step 1: Create One Defualt VPC for Bastion Host
```javascript
- Go to the Management Console
- Open VPC Console
- Jump to Your VPC section
- Go to Actions and Click on Create Defualt VPC
- Once Created, Rename the VPC name as Bastion
- Rename all the required components of VPC as you want
```
### Step2: Create Bastion Host
```javascript
- Jump to Cloud9 console
- Click on Create Environment, and put all the rquired details. Make sure you select the default VPC what you have created in the previous step and Amazon Linux Image for your host
```
### Step 3: Configure AWS Credentials for AWS Authentication

#### Two ways you can configure AWS Credentials
```javascript
- Create IAM user with only Programmatic Access
- Use your SSO User temporary Credentials
```
#### 1. Create IAM user
```javascript
- Go to IAM Console and Create One user having access to the AWS Services
- Once done, please open Bastion and move to settings
- Click on AWS Settings option and disable AWS managed credential option
- Jump to Bastion CLI and type the command AWS Configure
- Once you setup the credential, you are ready to create the EKS cluster
```
#### 2. Use your SSO User temporary Credentials
```javascript
- Go to IAM Console and Create One user having access to the AWS Services
- Once done, please open Bastion and move to settings
- Click on AWS Settings option and disable AWS managed credential option
- Copy your AWS temporary credentials mentioned at your login page via SSO
- Configure the credentials using Bastion CLI
```
### Step 4: Create Required IAM Execution Role

#### 1. IAM Role for EKS Cluster
Please attach the below policy to the IAM Role

```javascript
AmazonEKSClusterPolicy
```

#### 2. IAM Role for AWS Managed Nodes
Please attach the below policies to the IAM Role

```javascript
AmazonEKSWorkerNodePolicy
AmazonEC2ContainerRegistryReadOnly
AmazonEKS_CNI_Policy
```
### Step 5: Create VPC for EKS Cluster
```javascript
- Create the VPC with 3 Availability Zones, 3 Public Subnets, 3 Private Subnets, 3 NAT Gateways (You can keep one also, as its Chargeable)
```
### Step 6: Create EKS Cluster
```javascript
- Jump to EKS Console
- Click on Create Cluster
- Fill all the required fields with your values and click on Create. Make sure you select private subnets for the cluster to be created and click on the option Public & Private type cluster
- Wait for almost 10-20 Mins till the cluster comes to the Active state
```

### Step 7: Install Kubectl Tool

[Click me to install Kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

### Step 8: Add the K8s cluster context in ~/.kube/config file

Run the below command into your terminal to Configure your Bastion to establlish communication with K8s Cluster 

```javascript
aws eks update-kubeconfig --region region-code --name my-cluster
```

Once done, you run the below commands to test if your cluster is successfully running

Test your configuration

```javascript
kubectl get svc
```

Output should be as below:

```javascript
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
svc/kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   1m
```

### Step 9: Create Node Group for your Workload

```javascript
- Jump to EKS Console
- Select the cluster you just created
- Jump to Compute section
- Click on Add Node Group
- Fill all the required details and Create
- Wait for the node group to become active
- Once done, Please run the below commands if your node group is working fine.
```
View your cluster nodes
```javascript
kubectl get nodes -o wide
```
View the workloads running on your cluster
```javascript
kubectl get pods -A -o wide
```

## Cleanup

Once you perform all the above steps, Please make sure you cleanup all the resources as below:

```javascript
1. Delete the workloads you have created if any
2. Delete the Node Group
3. Delete the cluster
4. Delete the NAT Gateways
5. Diassociate the elastic IP addresses
6. Delete the EKS VPC
7. Delete the Bastion Environment
8. Delete the default VPC
```
## 🔗 Documentation

[AWS EKS Official Documentation](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html)

[Kubectl installation Guide](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
