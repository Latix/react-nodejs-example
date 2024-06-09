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

        stage("build jar") {
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
                    echo "Deploying the application"
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