pipeline {
    agent any
	environment {
	    AWS_ACCOUNT_ID="982722760872"
		AWS_DEFAULT_REGION="us-east-1"
		IMAGE_REPO_NAME="devopshint"
		IMAGE_TAG="latest"
		REPOSITORY_URL= "982722760872.dkr.ecr.us-east-1.amazonaws.com/devopshint"
	}
	
	stages {
	
	     stage('Logging into AWS ECR') {
		    steps {
			    script {
				sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
				}
			}
		 }
		 
		 stage('Cloning Git') {
		     steps {
			     checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/RAMDEVOPS23/ECR-PIPELINE-RAM.git']])
			 }
		 }
		 
		 stage('Building Image') {
		   steps {
		     script {
			   dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
			 }
		   }
		 }
		 
		 stage('Pushing to ECR') {
		   steps {
		       script {
			          sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URL}:$IMAGE_TAG"""
					  sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
			  
			  }
		   }
		 }
	}
}
