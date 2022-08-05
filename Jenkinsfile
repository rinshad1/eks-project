pipeline{
	
	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred')
		imageName = "rinshad11/hello_world_app:latest-v4"
		ClusterName = "education-eks-NEAhyIJ6"
		Region = "us-east-1"
		}
	agent any
	stages {

		stage('Build') {

			steps {
				sh 'docker build -t $imageName ./nodejsapp/.'
			}
		}
		
		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push $imageName'
			}
		}
		stage('Remove Unused docker image') {
      			steps{
         			sh 'docker rmi $imageName'
			}
    		}		
		
		stage('Adding nodes') {

			steps {
				sh 'aws eks update-kubeconfig --name $ClusterName --region $Region'
               
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
