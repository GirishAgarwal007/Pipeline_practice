pipeline {
          agent {
			label "Slave1"
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
				sh 'sudo docker build -t my-java-app:$BUILD_TAG . '
				SH 'sudo docker tag my-java-app:$BUILD_TAG girishagarwaldevops/my-java-app:$BUILD_TAG '
                        }
                }
          }
}

