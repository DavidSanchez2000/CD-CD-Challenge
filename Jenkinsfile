pipeline {
   agent {label "host"}
   environment {
       dockerHub = "docker.io"
       docker_cred = 'dockerhub'
   }
   stages {
		stage('SCM Checkout'){
			steps{
				checkout([$class: 'GitSCM',branches: [[name: 'origin/main']],extensions: [[$class: 'WipeWorkspace']],userRemoteConfigs: [[url: 'https://github.com/DavidSanchez2000/CI-CD-Challenge.git']]  ])
			}
		}
		stage("Build Docker Image"){
			steps{
				sh 'pwd'
				sh "docker build -t davidsanchez21/server_minecraft . --no-cache"
			}
		}
		stage('Upload Image to DockerHub'){
			steps{ 
	     	    withCredentials([
     		 	[$class: 'UsernamePasswordMultiBinding', credentialsId: docker_cred, usernameVariable: 'dockeruser', passwordVariable: 'dockerpass'],
  				]){
					//sh "sudo docker login -u ${dockeruser} -p ${dockerpass} ${dockerHub}"
					sh "echo \"$dockerpass\" | docker login -u \"$dockeruser\" --password-stdin"
  				}
	    	  	sh 'docker push davidsanchez21/server_minecraft'
	    	 }
	  	}
		stage("Running Docker Image"){
			steps{
                echo 'Running docker image'
                withCredentials([
     		 	[$class: 'UsernamePasswordMultiBinding', credentialsId: docker_cred, usernameVariable: 'dockeruser', passwordVariable: 'dockerpass'],
  				]){
					//sh "sudo docker login -u ${dockeruser} -p ${dockerpass} ${dockerHub}"
					sh "echo \"$dockerpass\" | docker login -u \"$dockeruser\" --password-stdin"
  				}
                sh 'docker pull davidsanchez21/server_minecraft'
                sh 'docker run -d -p 3307:3307 davidsanchez21/server_minecraft'
                
			}
		}
    }
}