pipeline {

    agent any

    stages {

        stage('Init') {

            steps {

                sh ''' 
                docker network create jenk-network || echo "Network already exists!"
                docker stop flask-app || echo "flask-app not running"
                docker stop nginx || echo "nginx not running"
                docker rm flask-app || echo "flask-app not running"
                docker rm nginx || echo "nginx not running"
                '''

            }

        }

        stage('Build') {

            steps {

                sh'''
                docker build -t arvindsiva68/flask-jenk:latest -t arvindsiva68/flask-jenk:v${BUILD_NUMBER} .
                docker build -t arvindsiva68/nginx-jenk:latest -t arvindsiva68/nginx-jenk:v${BUILD_NUMBER} ./nginx
                '''

            }

        }
                stage('Push') {

            steps {

                sh'''
                docker push arvindsiva68/flask-jenk:latest
                docker push arvindsiva68/flask-jenk:v${BUILD_NUMBER}
                docker push arvindsiva68/nginx-jenk:latest
                docker push arvindsiva68/nginx-jenk:v${BUILD_NUMBER}
                '''

            }

        }

        stage('Deploy') {

            steps {

                sh'''
                docker run -d --name flask-app --network jenk-network arvindsiva68/flask-jenk
                docker run -d -p 80:80 --name nginx --network jenk-network arvindsiva68/nginx-jenk
                '''

            }

        }

        stage('Cleanup') {
            steps {

                sh '''
                docker system prune -f
                '''

            }
        }

    }

}