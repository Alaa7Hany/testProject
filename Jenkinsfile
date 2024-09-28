pipeline {
    agent any

    environment {
        APP = '3laaharrrr/testproject:v3'
    }
    
    stages {
        
        stage('build image') {
            steps {
                script {
                    echo 'Building image......'
                    sh "docker build -t ${APP} ."
                    echo 'Image built'
                }
            }
        }

        stage('push image') {
            steps {
                script {
                    echo "pushing image...."
                    withCredentials([
                        usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')
                     ]) {
                            sh "echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin"
                            sh "docker push ${APP}"
                        }
                    echo 'image pushed'    
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    def dockerCmd = "docker run -p 8080:80 -d ${APP}"
                    sshagent(['ec2-docker-ssh']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@13.37.244.127 ${dockerCmd}"
                    }
                    echo 'Application deployed'
                }
            }
        }

    }
}
