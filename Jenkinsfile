pipeline {
          agent {
			label "slave2"
	}

          stages {
                stage ( "Please pull the code from github" ) {
                        steps {
                                git branch: 'main', url: 'https://github.com/GirishAgarwal007/Pipeline_practice.git'
                        }
                }
                stage ( "package building" ) {
                        steps { 
                                sh 'sudo mvn dependency:purge-local-repository' 
				sh 'sudo mvn clean package'
                        }
                }
                stage ("Building the Docker image" ) {
			steps {
				sh 'sudo docker build -t java-app:$BUILD_TAG . '
				sh 'sudo docker tag java-app:$BUILD_TAG girishagarwaldevops/java-app:$BUILD_TAG '
                        }
                }
		stage ("Please Push the  Docker Image to Docker Hub"){
			steps {
				withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
                                      	sh 'sudo docker login -u ${dockeruser} -p ${dockerpass}'
    					sh 'sudo docker push girishagarwaldevops/java-app:$BUILD_TAG'
}
                        }
	 	 }
		stage (" Testing the pipeline" ){
				steps {
						sh 'sudo docker run -dit -p 8080:8080 girishagarwaldevops/java-app:$BUILD_TAG'
				}
			}
			stage("testing website") {
				steps {
					retry(5) {
						script {
							sh 'curl --silent http://172.31.46.88:8080/java-web-app/ | grep -i -E "(india|sr)" '
							}
						}
					}				
			}
			stage("Approval status") {
				steps {
					script {
						 Boolean userInput = input(id: 'Proceed1', message: 'Do you want to Promote this build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                				echo 'userInput: ' + userInput
					}
				}
			}
                	stage("Prod Env") {
				steps {
					sshagent(['product']) {
					sh "ssh -o StrictHostKeyChecking=no ubuntu@35.154.193.74 sudo apt install -y docker.io"
					sh "ssh -o StrictHostKeyChecking-no ubuntu@35.154.193.74 sudo systemctl enable --now docker"
			    	 	sh "ssh -o StrictHostKeyChecking=no ubuntu@35.154.193.74 sudo docker run  -d -p 8080:8080  girishagarwal/java-app:$BUILD_TAG"
				}
			}
		}
          }
}

