#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@react', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/Latix/jenkins-shared-library',
    credentialsId: 'github-credential']
)

def gv

pipeline {
    agent any
    
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

        stage("build") {
            steps {
                script {
                    echo "Building the application"
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    echo "Building the image"
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    def dockerCmd = 'docker stop react-app && docker rm react-app && docker run -p 3000:80 -d --name react-app weridcoder/react-app:1.0.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.17.141.248 ${dockerCmd}"
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