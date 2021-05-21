# ECS-jenkins-scripted-Pipeline
This is the jenkins  scripted pipeline to pull your code from Github and push the same in ECS after ECR push.

<h2>Replace the below variables in Jenkinfile:</h3>

\<your-Email-ID-for-Approval> --> email ID who will approve the deployment <br />
\<Sender-Email-ID> --> From email ID <br />
\<your-credentials-Id> --> your git credential id ( you will have to save it in jenkins global credential) <br />
\<ECR-URI> --> your ECR URI <br />
\<ECR-name> --> Your ECR repo name <br />
\<your-ECS-Cluster-name> --> your ECS cluster name <br />
\<your-ECS-service-name> --> your service name <br />
  
