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
        IMAGE_NAME = 'simonknudsen7/demo-app:java-maven-1.0'
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
                    def dockerCmd = 'docker run -p 3080:3080 -d simonknudsen7/demo-app:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@13.60.229.184 ${dockerCmd}"
                    }
                }
            }
        }               
    }
} 
