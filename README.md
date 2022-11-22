# Webserver-hosting

This project features a static webpages application deployed as workloads on AWS Managed EKS cluster.
The static web application consists of two web pages hosted on its particular nginx webserver which is authenticated through users credentials.

Tools been utilized for the process - </br>
**[Nginx Webserver]** - Using Nginx configurations defined to host the index page on the nginx alpine base image. </br>

**[Docker]** - Which is used to build and publish image to the Docker Hub Repository. </br>

**[Jenkins]** - As an automation pipeline to build the images, publish it on the dockerhub repo and update the build instance image name in helm chart. </br>

**[Terraform]** - For spinning up EKS cluster and corresponding the supporting VPC modules in AWS. </br>

**[EKS]** - RUnning on us-west-2 region.</br>

**[Helm Charts]** - This bascially bundles or package up the resource needed for building up the application. It also basically links the kubernetes objects which has been deployed for the kubernetes cluster.</br>

**[ArgoCD}** - A tool for automating helm charts deployment on K8s cluster.

<h3> Creating AWS VPC and EKS cluster </h3>

**Provide AWS Credentials**

```
$ aws configure:
$............... AWS Access Key ID [None]:
$............... AWS Secret Access Key [None]:
$............... Default region name [None]:
$............... Default output format [None]:
```

Initialize and pull terraform cloud specific dependencies:
```
$ terraform init
```
View terraform plan:
```
$ terraform plan
```
Apply terraform plan:
```
$ terraform apply
```

Terraform modules will create
```
-   VPC
-   Subnets
-   Routes
-   IAM Roles for master and nodes
-   Security Groups "Firewall" to allow master and nodes to communicate
-   EKS cluster
-   Managed nodes groups to be added to the cluster
```

Executing the command will prompt the kubeconfig file to be loaded within your home .kube directory
```
aws eks --region us-west-2 update-kubeconfig --name my-cluster
```
<h3> Nginx Webserver COnfiguration </h3> 
The nginx configuration consists of a basic method to host the static page on default server on port 80.

<h2> Jenkins CI/CD for creating webservers docker image </h2> 

The Jenkins file will clone the repo and create docker images with the version being the Job Build instance ```${DOCKER_REG}/${IMAGE_NAME}:${BUILD_NUMBER}``` and will publish into the docker hub repo and will update the job instance build image name into values.yaml helm chart.

<img width="317" alt="Jenkins-image-creation-1" src="https://user-images.githubusercontent.com/33144027/203221915-92a91c50-9a99-4d81-a8df-933ef27de2a7.PNG">
<img width="321" alt="Jenkins-image-creation-2" src="https://user-images.githubusercontent.com/33144027/203221945-854e0bab-dc5c-48e4-8891-487a137c240f.PNG">


