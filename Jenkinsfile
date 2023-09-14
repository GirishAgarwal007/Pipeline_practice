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
		stage ("Pushing Docker Image to Docker Hub"){
			steps {
				withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
                                      	sh 'sudo docker login -u ${dockeruser} -p ${dockerpass}'
    					sh 'sudo docker push girishagarwaldevops/java-app:$BUILD_TAG'
}
                        }
	 	 }
          }
}

