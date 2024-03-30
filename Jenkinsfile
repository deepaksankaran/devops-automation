pipeline {
    agent any
    tools {
        maven 'maven_3_5_0'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/deepaksankaran/devops-automation']]])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    bat 'docker build -t javatechie/devops-integration .'
                }
            }
        }
        stage('Push image to Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '1d6bcda3-8e6f-4dd5-b6ba-4a6e3d457d67', usernameVariable: 'sdeepak1008', passwordVariable: 'Sreesailam')]) {
                        bat "docker login -u ${sdeepak1008} -p ${Sreesailam}"
                        bat 'docker tag javatechie/devops-integration:latest sdeepak1008/devops-integration:latest' // Tag the image
                        bat 'docker push sdeepak1008/devops-integration:latest'
                    }
                }
            }
        }
    }
}

