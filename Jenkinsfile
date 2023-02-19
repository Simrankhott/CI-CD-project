pipeline {
    agent any 
    environment {
        VERSION = "${env.BUILD_ID}"
        KUBECONFIG = credentials('kconfig-secret')
}
    }
    stages {
        stage("sonar quality check") {
            agent {
                docker {
                    image 'maven'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage("docker build & docker push to nexus repo") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                        sh '''
                          docker login -u admin -p ${nexus_creds} 13.233.131.100:8083
                          docker build -t 13.233.131.100:8083/springapp:${VERSION} .
                          docker push 13.233.131.100:8083/springapp:${VERSION} 
                          docker rmi 13.233.131.100:8083/springapp:${VERSION} 
                         '''
                    }
                }
            }
        }
        stage('Identifying misconfigs using datree in helm charts') {
            steps {
                script {
                    dir('kubernetes/') {
                        withEnv(['DATREE_TOKEN=2e7eeda6-aeae-4d04-9ce1-5fd0f8e5edaf']) {
                            sh 'helm datree test myapp/'
                        }
                    }    
                }
            }
        }        
        stage("Pushing the helm charts to nexus") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                         dir('kubernetes/') {
                            sh '''
                               helmversion=$(helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ')
                               tar -czvf myapp-${helmversion}.tgz myapp/
                               curl -u admin:$nexus_creds http://13.233.131.100:8081/repository/helm-hosted/ --upload-file myapp-${helmversion}.tgz -v
                             '''
                        }
                    }
                }
            }
        }    
        stage('Deploying application on k8s cluster') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'kconfig-secret', variable: 'KCONFIG')]) {
                        dir('kubernetes/') {
                            sh 'cp $KCONFIG kconfig.txt'
                            sh 'export KUBECONFIG=$KUBECONFIG:kconfig.txt'
                            sh 'helm upgrade --install --set image.repository="34.125.214.226:8083/springapp" --set image.tag="${VERSION}" myjavaapp myapp/'
                        }
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

