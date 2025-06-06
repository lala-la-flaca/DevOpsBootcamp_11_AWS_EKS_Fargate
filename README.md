# AWS EKS
## 📦 Demo 2
This demo is part of **Module 11: Kubernetes on AWS (EKS)** in the **TWN DevOps Bootcamp**. The goal is to manually create a **Kubernetes cluster** using **Amazon Elastic Kubernetes Service (EKS)** and deploy a Fargate profile using the AWS Console.

## 🚀 Technologies Used
- **Amazon EKS**: Amazon Elastic Kubernetes Service.
- **Amazon EC2**: Compute Instances used for worker nodes.
- **I AM**: AWS identity service to manage access and secure permissions.
- **kubectl**: CLI to interact with Kubernetes.
- **AWS CLI**: Interface for managing AWS services
- **CloudFormation**: To manage the VPC template.
- **AWS Fargate**: for fargate profile nodes
  
## 📋 Prerequisites
- Ensure you have an AWS Account.
- Ensure AWS CLI is installed and configured.
- Kubectl is installed and configured to connect to the Kubernetes cluster.
- Demo 1 is running.
  

## 📌 Objective
Set up a fully managed Kubernetes environment using **Amazon EKS** and deploy a **Fargate Profile**  to serve as worker nodes.

## 🎯 Features
- Configure Fargate IAM Roles.
- Create Fargate Profile.
- Deploy a sample application to the EKS Cluster.


## 🏗 Project Architecture

<img src=""/>


## ⚙️ Project Configuration
### Creating an IAM Role for Fargate Profile
1. Open the AWS console.
   
2. In the search bar, type IAM, and select Roles.
   
3. Select Create Role.
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS/blob/main/Img/1%20aws%20console%20role.png" width=800 />
   
4. Under Trusted entity type, choose AWS service.
   
5. Select Elastic Kubernetes Service (EKS) and  EKS- Fargate pod.
    
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/1%20creating%20fargate%20role.png" width=800 />
   
6. Follow the prompts to complete the configuration.
    
7. Select Create Role to finish.

   
### Creating FargateProfile
1. In the AWS Console, open the EKS service and select the cluster you created.
   
2. Under the Compute section, click Add Fargate Profile.
   This step creates managed compute capacity for the EKS cluster without requiring EC2 worker nodes

  <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/2%20creating%20fargate%20nodes.png" width=800 />

3. Configure the Fargate Profile:
    * Enter a name for the profile
    * Select the IAM role created in the previous step.
    * Select the available subnets within the VPC.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/3%20enter%20name%20role%20subnets%20for%20fargate%20profile.png" width=800 />
      
4. On the pod selection page, define how Fargate should identify pods to schedule:
     namespace: dev
     label: profile:fargate
    The combination of the Namespace and labels ensures that only matching pods are scheduled on Fargate.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/6%20namespace%20and%20labels%20%20aws%20fargate%20profile.png" width=800/>

5. Follow the prompts to complete the configuration.
   
### Deploying the Nginx app
1. Update the deployment.yaml file to include:
     * namespace:dev   
     * key pair label: profile:fargate
   
   These settings ensure the pod is scheduled on Fargate.
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/5%20modify%20the%20depoyment%20nginx%20file%20to%20add%20labels%20for%20fargate.png" width=800 />
   
3. Verify the current pods.
   ```bash
   kubectl get pod
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/7%20current%20status%20pods%20deployed%20in%20nodegroup.PNG" width=800/>
   
5. Verify existing namespaces.
   ```bash
   kubectl get namespaces
   ```
   
7. Create the dev namespace before applying the deployment file.
    Kubernetes requires the namespace to exist before resources can be deployed into it.
   ```bash
   kubectl create ns dev
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/8%20%20chcking%20current%20ns%20and%20creating%20dev%20ns%20before%20deploying%20pods.png" width=800 />
   
9. Apply the deployment using kubectl.
    ```bash
    kubectl apply -f nginx-config.yaml
    ```
11. Check pods in the dev namespace.
    ```bash
    kubectl get pods -n dev
    ```
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/9%20deploying%20nginx%20pods%20using%20dev%20ns%20and%20fargate.png" width=800 />
    
13. Verify nodes. You should see both your managed node group and Fargate nodes. In Fargate, each pod runs on its own dedicated virtual node, whereas with EC2 node groups, multiple pods share the same node.
    ```bash
    kubectl get nodes
    ```
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS_Fargate/blob/main/Img/11%20current%20nodes%20fargate%20%2B%20nodegroup%20Ec2.PNG" width=800 />
    




