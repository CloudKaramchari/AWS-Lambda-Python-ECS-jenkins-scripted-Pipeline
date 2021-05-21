# ECS-jenkins-scripted-Pipeline
This is the jenkins  scripted pipeline to pull your code from Github and push the same in ECS after ECR push.

Replace the below variables in Jenkinfile:

<your-Email-ID-for-Approval> --> email ID who will approve the deployment
<Sender-Email-ID> --> From email ID
<your-credentials-Id> --> your git credential id ( you will have to save it in jenkins global credential)
<ECR-URI> --> your ECR URI
<ECR-name> --> Your ECR repo name
<your-ECS-Cluster-name> --> your ECS cluster name
<your-ECS-service-name> --> your service name
