#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@react', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/Latix/jenkins-shared-library',
    credentialsId: 'github-credential']
)

def gv

pipeline {
    agent any
    environment {
        IMAGE_NAME = ' weridcoder/react-app:1.0.1'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage("test") {
            steps {
                script {
                    buildTest()
                }
            }
        }

        stage("build image & push to repo") {
            steps {
                script {
                    echo "Building the image..."
                    dockerLogin()
                    buildImage(env.IMAGE_NAME)
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                     echo "Deploying docker image to EC2..."
                    def dockerCmd = "docker run -p 3000:80 -d ${IMAGE_NAME}"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.145.149.86 ${dockerCmd}"
                        // sh "docker stop react-app"
                        // sh "docker rm react-app"
                        // sh "${dockerCmd}"
                    }
                }
            }
        }
    }
    post {
        always {
             echo 'Notified'
        }
        failure {
             echo 'Failed'
        }
        success {
            echo 'Success'
        }
    }   
}