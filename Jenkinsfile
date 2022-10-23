pipeline {
environment {
    dockerimagename = "firozehossain01/spring-boot-app"
    dockerImage = ""
    }
    agent {label 'master'}
    tools{
        maven 'maven_3.8.6'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/firoze-hossain/devops-integration']]])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                dockerImage = docker.build dockerimagename
                }
            }
        }
        stage('Push image to Hub'){
        environment{
        registryCredential = 'dockerhublogin'
        }
            steps{
                script{
                    docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
                               dockerImage.push("latest")

}
               }
           }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubernetes')
                }
            }
        }
    }
}