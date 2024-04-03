pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
              git branch: 'main', url: 'https://github.com/srankmeng/workshop-docker-cicd-20240403-04.git'
            }
        }
        stage('Code analysis') {
            steps {
                echo 'Code analysis'
            }
        }
        stage('Unit test') {
            steps {
                echo 'Unit test'
            }
        }
        stage('Code coverage') {
            steps {
                echo 'Code coverage'
            }
        }
        stage('Build images') {
            steps {
                sh 'docker compose -f ./java/docker-compose.yml build'
            }
        }
        stage('Setup & Provisioning') {
            steps {
                sh 'docker compose -f ./java/docker-compose.yml up -d'
            }
        }
        stage('Run api automate test') {
            steps {
                sh 'docker compose -f ./newman/docker-compose.yml build'
                sh 'docker compose -f ./newman/docker-compose.yml up'
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub'
                , passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh '''docker image tag java:1.0 srank123/my_java:$BUILD_NUMBER
                          docker image push srank123/my_java:$BUILD_NUMBER'''
                }        
            }
        }
    }
    post {
        always {
            sh 'docker compose -f ./java/docker-compose.yml down'
        }
        success {
            script {
                def currentBuildNumber = env.BUILD_NUMBER
                build job: 'demo_deploy_pipeline', parameters: [string(name: 'IMAGE_TAG', value: "${currentBuildNumber}")]
            }
        }
    }
}
