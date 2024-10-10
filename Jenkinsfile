pipeline {
    agent any

    environment {
        APP = '3laaharrrr/testproject'
        VERSION = 'v4'
        EC2_IP = '15.188.59.142'
        EMAIL = '3laahany946@gmail.com'
    }
    
    stages {
        
        stage('build image') {
            steps {
                mail bcc: '', body: 'Hello, This is an email from jenkins pipeline.', cc: '', from: '', replyTo: '', subject: 'EmailJenkinsPipeline', to: '3laahany946@gmail.com'
                script {
                    
                    echo 'Building image......'
                    sh "docker build -t ${APP}:${VERSION} -t ${APP}:latest ."
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
                            sh "docker push ${APP}:${VERSION}"
                            sh "docker push ${APP}:latest"
                        }
                    echo 'image pushed'    
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    //def dockerCmd = "docker run -p 8080:80 -d ${APP}"

                    sshagent(['ec2-docker-ssh']) {
                        "Copping docke-compose.yaml........"
                        sh """
                            scp -o StrictHostKeyChecking=no docker-compose.yaml ubuntu@${EC2_IP}:/home/ubuntu/
                        """
                        sh """
                            ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} 'cd /home/ubuntu/ && docker-compose down || true && docker-compose pull && docker-compose up -d'
                        """
                    }
                    echo 'Application deployed'
                }
            }
        }

    }
    post {
        success {
            emailext (
                to: "${EMAIL}",
                subject: "Deployment Successful: Jenkins Build ",
                body: '''The deployment to the EC2 instance at ${EC2_IP} was successful.
                
                Job URL: 
                '''
            )
        }
        failure {
            emailext (
                to: "${EMAIL}",
                subject: "Deployment Failed: Jenkins Build ",
                body: '''The deployment to the EC2 instance at ${EC2_IP} failed.
                
                Job URL: 
                '''
            )
        }
    }
}
