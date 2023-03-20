//pipeline {
 //   agent any
 //   stages {
  //      stage('Build') {
   //         steps {
   //             bat 'mvn clean package'
  //          }
   //     }
   // }
//}
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        def app = docker.build("your-dockerhub-username/your-webapp:${env.BUILD_NUMBER}", ".")
                        app.push()
                    }
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        docker.login('your-dockerhub-username', credentialsId: 'dockerhub')
                    }
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        docker.image("your-dockerhub-username/your-webapp:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }
}
