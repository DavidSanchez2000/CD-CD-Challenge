pipeline {
   agent any
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
				sh "docker build -t davidsanchez21/server_minecraft ."
			}
		}
		stage('Upload Image to DockerHub'){
			steps{ 
	     	    withCredentials([
     		 	[$class: 'UsernamePasswordMultiBinding', credentialsId: docker_cred, usernameVariable: 'dockeruser', passwordVariable: 'dockerpass'],
  				]){
					sh "docker login -u ${dockeruser} -p ${dockerpass} ${dockerHub}"
  				}
	    	  	sh 'docker push davidsanchez21/server_minecraft'
	    	 }
	  	}
		stage("Running Docker Image"){
			steps{
                echo 'Running docker image'
                sshagent(credentials: ['ssh-key-1']) {
                    sh 'ssh -o StrictHostKeyChecking=no vagrant@192.168.33.11 sudo docker run -d -p 3307:3307 davidsanchez21/server_minecraft'
				}
			}
		}
    }
}