#!/usr/bin/env groovy

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
                    echo "Testing the application"
                    echo "Executing pipeline for ${BRANCH_NAME}"
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
                    def dockerCmd = 'docker run -p 3000:80 -d weridcoder/react-app:1.0.0'
                    
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