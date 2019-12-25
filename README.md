# What to do?

## Creating a Kubernetes Cluster

Source: https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md

1- Create a Ubuntu Server instance on AWS. It will be used to configure the Kubernetes cluster.

Check this: https://github.com/ValaxyTech/DevOpsDemos/blob/master/Kubernetes/k8s-setup.md

Assign to the Role of the VM the following policites:

-`AmazonEC2FullAccess`

-`IAMFullAccess`

-`AmazonRoute53FullAccess`

-`AmazonS3FullAccess`

-`AmazonVPCFullAccess`

2- To get the zones name

`aws ec2 describe-availability-zones --region us-east-1`

3- Create the kubernetes Cluster

`kops create cluster --cloud=aws --zones=us-east-1a  --name=YOURUSERNAME.cluster.k8s.local --state=s3://YOURBUCKET --node-count=3 --node-size=t2.micro --master-size=t2.micro`

You change change node-count here.

4- Update the Kubernetes Cluster

`kops update cluster YOURUSERNAME.cluster.k8s.local --state=s3://YOURBUCKET --yes`

5- Check the Kubernetes Cluster

`kops get cluster --state=s3://YOURBUCKET`

6-check if cluster is working fine

`kops validate cluster`

NB: `cluster.k8s.local` is used to create clusters without a DNS.

## Deleting a Cluster

get the cluster name

`kubectl config current-context`

`kubectl config delete-cluster CLUSTER_NAME`

or

`kops delete cluster --state=s3://YOURBUCKET --name YOURUSERNAME.cluster.k8s.local --yes`

## Configuring the Envirnment

`kubectl apply -f env-configmap.yaml`

`kubectl apply -f env-secret.yaml`

`kubectl apply -f aws_secret.yaml`			# update credentials here vi .aws/credentials

## Starting the pods and services

`kubectl apply -f backend_feed.yaml`

`kubectl apply -f backend_user.yaml`

`kubectl apply -f backend-user-service.yaml`

`kubectl apply -f backend-feed-service.yaml`

`kubectl apply -f pod.yaml`						# Testing a Pod (run this if you want to test everything) 

`kubectl delete pod pod-example`					# Run this only if you decided to test the pod.yaml

`kubectl apply -f frontend-deployment.yaml`

`kubectl apply -f frontend-service.yaml`

`kubectl apply -f reverseproxy-deployment.yaml`

`kubectl apply -f reverseproxy-service.yaml`

## Delete a replicaset

`kubectl delete -f backend_feed.yaml`

## Show logs of a pod

`kubectl logs pod-example`


## list of pods (name and IP)

`kubectl get pods  -o wide`

## Run a command inside a pod

`kubectl exec -it NAMEOFTHEPOD -- ifconfig`

## list of services

`kubectl get services`

## Expose the application to the public

`kubectl expose deployment NAMEOFSERVICETOEXPOSE --type=LoadBalancer --name=my-service-exposed`

## Forward a port

`kubectl port-forward service/reverseproxy 8080:8080 &`

`kubectl port-forward service/frontend 8100:8100 &`

`kubectl port-forward service/backend-user 8080:8080 &`

`jobs` # to see the jobs in the bg

`fg` # bring a job to the foreground

## Scaling Deployments

`kubectl get deployments`

`kubectl scale deployment/backend-feed --replicas=0` # scale down

`kubectl scale deployment/backend-feed --replicas=1` # scale up

