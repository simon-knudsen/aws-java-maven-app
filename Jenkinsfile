#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM([
    $class       : 'GitSCMSource',
    remote       : 'https://github.com/simon-knudsen/jenkins-shared-library.git',
    credentialsId: 'github-credentials'
])

pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }

    tools {
        maven 'maven-3.10'
    }

    environment {
        IMAGE_NAME     = 'simonknudsen7/demo-app:java-maven-1.0'
        CONTAINER_NAME = 'demo-app'
        EC2_HOST       = 'ec2-user@13.60.229.184'
        HOST_PORT      = '3080'
        APP_PORT       = '8080'
    }

    stages {
        stage('Build app') {
            steps {
                echo 'Building application jar...'
                buildJar()
            }
        }

        stage('Build image') {
            steps {
                echo 'Building and pushing the Docker image...'
                buildImage(env.IMAGE_NAME)
                dockerLogin()
                dockerPush(env.IMAGE_NAME)
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${env.IMAGE_NAME} to ${env.EC2_HOST}..."
                sshagent(['ec2-user']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no "$EC2_HOST" "
                            docker pull $IMAGE_NAME &&
                            (docker rm -f $CONTAINER_NAME || true) &&
                            docker run -d --name $CONTAINER_NAME -p $HOST_PORT:$APP_PORT $IMAGE_NAME
                        "
                    '''
                }
            }
        }
    }
}
