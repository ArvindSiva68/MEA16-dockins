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
                docker build -t flask-jenk .
                docker build -t nginx-jenk ./nginx
                '''

            }

        }

        stage('Deploy') {

            steps {

                sh'''
                docker run -d --name flask-app --network jenk-network flask-jenk
                docker run -d -p 80:80 --name nginx --network jenk-network nginx-jenk
                '''

            }

        }

        stage('Cleanup') {
            steps {

                sh '''
                docker system prune -f
                echo "prune time"
                '''

            }
        }

    }

}