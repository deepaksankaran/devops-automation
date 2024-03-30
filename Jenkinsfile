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
                        bat 'docker tag sdeepak1008/devops-integration:latest sdeepak1008/devops-integration:1.0.0' // Tag the image explicitly
                        bat 'docker push sdeepak1008/devops-integration:latest'
                    }
                }
            }
        }
        stage('Deploy to k8s') {
            steps {
                script {
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
