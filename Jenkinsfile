node
{
	def jobName = currentBuild.fullDisplayName
	def mailToRecipients = '<your-Email-ID-for-Approval>'
	def useremail='<Sender-Email-ID>'
	
	stage('Pull Git code') 
	{
		git credentialsId: '<your-credentials-Id>', url: 'https://github.com/nakkxx/Java-application-demo.git'
	}
	
	stage('Build Docker file')
	{
		sh '/usr/bin/docker build -t java-application-server .'
	}
	stage('Change tagging to ECR pattern ')
	{
	
			sh '/usr/bin/docker tag java-application-server:latest <ECR-URI>/<ECR-name>:latest'
	}
	stage('Push to deploy in pre-prod') 
	{
		def userAborted = false
		emailext body: """Please go to console output of <a href="${BUILD_URL}input">Visit approval Console</a> to approve or Reject.<br>""", 
		mimeType: 'text/html',
		subject: "[Jenkins] ${jobName} Build Approval Request",
		from: "${useremail}",
		to: "${mailToRecipients}",
		recipientProviders: [[$class: 'CulpritsRecipientProvider']]

		try { 
		  userInput = input submitter: 'Vagrant', message: 'Do you approve?'
	} catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
	  cause = e.causes.get(0)
	  echo "Aborted by " + cause.getUser().toString()
			userAborted = true
		  echo "SYSTEM aborted, but looks like timeout period didn't complete. Aborting."
	}
	if (userAborted) {
		currentBuild.result = 'ABORTED'
	} else {
		 sh 'aws ecr get-login-password --region ap-south-1 | /usr/bin/docker login --username AWS --password-stdin <ECR-URI>'
	}
	
	}
	
	stage('Add Build version tag and Push docker in ECR')
	{
	   parallel 're-tagging-start': {
        stage('re-tag ECR Image') {
             sh '''#!/bin/bash 
                    MANIFEST=$(aws ecr batch-get-image --repository-name <ECR-name> --region ap-south-1 --image-ids imageTag=latest --query \'images[].imageManifest\' --output text)
                    
                    echo $MANIFEST
                    
                    aws ecr put-image --repository-name <ECR-name> --region ap-south-1 --image-tag lastest_Version_Build."${BUILD_NUMBER}" --image-manifest "$MANIFEST"'''
                    	   
        }
    }, 'push-to-ECR': {
        stage('Push Docker file in ECR') {
            
			sh '/usr/bin/docker push <ECR-URI>/<ECR-name>:latest'
        }
    }
   
	}
	//stage('Push Docker file in ECR')
	//{
			
    //}
    stage('Deploy in Pre-prod servers')
    {	
	 sh 'aws ecs update-service --cluster <your-ECS-Cluster-name> --service <your-ECS-service-name> --force-new-deployment --region ap-south-1'
    }
	stage('Delete old images from jenkin server')
	{
		sh '/usr/bin/docker images | grep \'java-application-server\' | awk \'{print $3}\' | xargs /usr/bin/docker rmi --force' 
	}
}
