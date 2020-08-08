# Capstone Project - Udacity Cloud DevOps Nanodegree

The final Capstone challenge for Udacity Cloud DevOps Nanodegree was to create a CI/CD Pipeline with Jenkins, ECR, EKS through Cloudformation or Ansible. This Project primarily deals with the Cloudformation scripts for the ECR, EKS the EKS Worker Nodes and the Network stack. 

## Learning Objectives

- Spinning up & Creating Jenkins
- Configuring Jenkins with the needed plugins and batteries (docker+kubectl)
- Pushing Docker images to an private ECR Instance (fast and easy)
- Pull Docker images from the private ECR Instance
- Deploy those images through k8s

## How to

The following steps are required to get this moving -

a) Create a network stack:
`sh create_stack.sh UdacityDevOpsNanodegreeCapStone-Infrastructure network.yml network-params.json`

b) Create K8s cluster:
`sh create_stack.sh UdacityDevOpsNanodegreeCapStone-K8S cluster.yml cluster-params.json`

c) Create K8s worker nodes:
`sh create_stack.sh UdacityDevOpsNanodegreeCapStone-K8S-Workers cluster-workers.yml cluster-workers-params.json`

d) A few quick tricks to set up auth, in the local machine:

Connect AWS EKS with local kubectl utility 
`aws eks --region eu-west-2 update-kubeconfig --name UdacityDevOpsNanodegreeCapStone-Cluster`

Apply AWS Auth
`kubectl apply -f aws-auth-cm.yaml`

Check service
`kubectl get svc`

Generate secret 
`EMAIL=sil.nilanjan@gmail.com
TOKEN=$(aws ecr --region=eu-west-2 get-authorization-token --output text --query authorizationData[].authorizationToken | base64 -d | cut -d: -f2)
kubectl delete secret --ignore-not-found ecr-secret	
kubectl create secret docker-registry ecr-secret --docker-server=627513304485.dkr.ecr.us-west-2.amazonaws.com --docker-username=AWS --docker-password=$TOKEN --docker-email=$EMAIL`

e) Create the ECR:

`sh create_stack.sh UdacityDevOpsNanodegreeCapStone-ECR container-registry.yml container-registry-params.json`

### Application

The application is a simple static site running in an nginx container exposing port 80.

### Deployment pipeline

To have a proper deployment double-check the parameters in environment in the `Jenkinsfile`

Rolling update is enabled, which means it updates the pods without any downtime. The rolling update updates one pod after another. If the first pod isn't successful K8s rolls it back and spins up another instance of the previous pod, without any downtime.

Indicated is the rolling update within the `k8s/deployment.yml`-file through the deploy strategy type. The CI/CD pipeline (`Jenkinsfile`) performs an rolling update command to update the image to the latest image created during the build.

