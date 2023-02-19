pipeline {
    agent any 
    environment {
                VERSION = "${env.BUILD_ID}"
                }
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
        
             stage ("docker build and docker push to nexus repo") {
                steps {
                    script{
                        withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                        sh '''
                          docker build -t 13.126.53.240:8083/springapp:${VERSION} .
                          docker login -u admin -p $nexus_creds 13.126.53.240:8083
                          docker push  13.126.53.240:8083/springapp:${VERSION} 
                          docker rmi  13.126.53.240:8083/springapp:${VERSION} 
                         '''
                        }
                    }
                }
             }
        }
    }
}

    


