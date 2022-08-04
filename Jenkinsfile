pipeline{
	
	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred')
		}
	agent any
	stages {

		stage('Build') {

			steps {
				sh 'docker build -t rinshad11/hello_world_app:latest-v4 ./nodejsapp/.'
			}
		}
		
		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push rinshad11/hello_world_app:latest-v4'
			}
		}
		stage('Remove Unused docker image') {
      			steps{
         			sh 'docker rmi -f $(docker image ls -a -q)'
			}
    		}		
		
		stage('Adding nodes') {

			steps {
				sh 'aws eks update-kubeconfig --name education-eks-yvM5xsHH --region us-east-1'
               
			}
		} 

        	stage('eks deploy') {

			steps {
				sh 'pwd'
				sh 'ls -l'
                		sh 'kubectl apply -f ./deployment/.'
				sh 'kubectl get nodes'
                		sh 'kubectl get pods'
				sh 'kubectl get svc'
			}
		}
	
	}
	post {
		always {
			sh 'docker logout'
		}
	}
	
}
