pipeline{
    agent any 
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    stages{
        stage('Git Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Ajjai99/automation.git']])
            }
        }

        stage('Build Maven Application'){
            steps{
                sh "mvn clean install"
            }
        }

        stage('Build Docker Image'){
            steps{
                script{
                    sh "docker build -t ajjai007/devops_automation_2 ."
                }
            }
        }

        stage('Push Docker Image to DockerHub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh "docker login -u ajjai007 -p ${dockerhubpwd}"
                        sh "docker push ajjai007/devops_automation_2"
                    }
                }
            }
        }

        stage('Run Docker Container'){
            steps{
                script{
                    sh "docker run -p 8082:8080 -d ajjai007/devops_automation"
                }
            }
        }
    }
}
