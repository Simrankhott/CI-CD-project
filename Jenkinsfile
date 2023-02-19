pipeline{
    agent any 
    environment{
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
        }
        stage("docker build & docker push to nexus repo"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                        sh '''
                          docker login -u admin -p password 15.207.112.200:8083
                          docker build -t 15.207.112.200:8083/springapp:${VERSION} .
                          docker push 15.207.112.200:8083/springapp:${VERSION} 
                          docker rmi 15.207.112.200:8083/springapp:${VERSION} 
                         '''
                        }
                    }
                }
            }
        }
        stage ("Identifying misconfigs using datree in helm charts"){
            steps{
                script{
                    dir('kubernets/app/') {
                        withEnv(['DATREE_TOKEN=2e7eeda6-aeae-4d04-9ce1-5fd0f8e5edaf']) {
                        sh 'helm datree test . '
                    } 
                }       
            }
        }
    }    
    post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "khotsimran04@gmail.com";  
		}
	}
}
