pipeline {
          agent any

          stages {
                stage ( "pull the code from github" ) {
                        steps {
                                git branch: 'main', url: 'https://github.com/GirishAgarwal007/Pipeline_practice.git'
                        }
                }
                stage ( "package building" ) {
                        steps {
                                sh 'mvn clean package'
                        }
                }
          }
}

