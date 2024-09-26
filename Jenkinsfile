pipeline {
    agent any
    stages {
        
        stage('build image') {
            steps {
                script {
                    echo 'Building image......'
                    sh 'docker build -t 3laaharrrr/hubgit:v1 .'
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
                            sh 'docker push 3laaharrrr/hubgit:v1'
                        }
                    echo 'image pushed'    
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    sh '''
                        docker run -d -p 85:80 3laaharrrr/hubgit:v1
                    '''
                    echo 'Application deployed'
                }
            }
        }

    }
}
