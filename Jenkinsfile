pipeline{
    agent any 
    stages{
        stage("sonar quality check"){
                agent {
                    docker {
                        image 'maven'
                    }
                }
                steps {
                    script{
                        withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'mvn clean package sonar:sonar'
                    }
                }
             }
        }
    }
}
           /*   stage ("docker build and docker push to nexus repo") {
                steps {
                    script{

                    }
                }
             }
        }
    }
} */
    


