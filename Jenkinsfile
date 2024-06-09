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
                    def dockerCmd = 'docker run -p 3000:80 -d weridcoder/react-app:1.0.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.128.181.33 ${dockerCmd}"
                        // sh "docker stop react-app"
                        // sh "docker rm react-app"

                        sh "${dockerCmd}"
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