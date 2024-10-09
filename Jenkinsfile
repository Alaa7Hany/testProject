pipeline {
    agent any

    environment {
        APP = '3laaharrrr/testproject'
        VERSION = 'v4'
    }
    
    stages {
        
        stage('build image') {
            steps {
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
                            scp -o StrictHostKeyChecking=no docker-compose.yaml ubuntu@13.36.37.129:/home/ubuntu/
                        """
                        sh """
                            ssh -o StrictHostKeyChecking=no ubuntu@13.36.37.129 'cd /home/ubuntu/ && docker-compose down || true && docker-compose pull && docker-compose up -d'
                        """
                    }
                    echo 'Application deployed'
                }
            }
        }

    }
}
