#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
        [$class: 'GitSCMSource',
        remote: 'https://github.com/simon-knudsen/jenkins-shared-library.git',
        credentialsId: 'github-credentials'
        ]
)

pipeline {
    agent any

    tools {
        maven 'maven-3.10'
    }

    environment {
        IMAGE_NAME = 'simonknudsen7/demo-app:java-maven-2.0'
    }

    stages {
        stage('build app') {
            steps {
                echo 'building application jar...'
                buildJar()
            }
        }

        stage('build image') {
            steps {
                script {
                    echo 'building the docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo 'deploying docker image to EC2...'

                    def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                    def ec2Instance = "ec2-user@13.60.229.184"

                    sshagent(['ec2-server-key']) {
                        sh "scp server-cmds.sh ${ec2Instance}:/home/ec2-user"
                        sh "scp docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                    }
                }
            }
        }               
    }
} 
