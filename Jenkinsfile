def commit_id
pipeline {
	agent any
	stages{
		stage('preparation'){
			steps{  
				checkout scm
				sh "git rev-parse --short HEAD > .git/commit-id"
				script {
				commit_id = readFile ('.git/commit-id').trim()
				}
			}	
		}

		stage('build'){
			steps{ 
				echo '1. Building Maven Workload'
				sh "mvn clean install"
				echo 'Build completed'
				}	
	 		}
		
		stage('image build'){
			steps{ echo '2. Building Docker image'
				sh "docker build -t 2alinfo7/position-simulator:${commit_id} ."
				echo 'Docker image built'}	
			
}
		stage ("image push") {
            		steps {
                		echo '3. Pushing Docker Image'
                		sh "docker push 2alinfo7/position-simulator:${commit_id} "
                		echo 'Docker image pushed'
            			}
        		}

		stage('deploy'){
			steps{ 
				echo '4. Deployment To Kubernetes'
				sh "sed -i -r 's|richardchesterwood/k8s-fleetman-webapp-angular:release2|2alinfo7/position-simulator:${commit_id}|' workloads.yaml"
				sh "kubectl apply -f workloads.yaml "
				echo "Deployment completed"	
				}
			}
		}
	} 

