#!groovy

def notifyBuild(String buildStatus) {
    buildStatus =  buildStatus ?: 'SUCCESSFUL'
    def summary = "${buildStatus}: '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
}

List servers = ['10.133.103.129']

pipeline {
    agent any

    environment {
        CHAT_URL = 'https://example.com/integration/jenkins'
        APP_PORT = 8051
        APP_NAME = 'demo3'
    }
    options {
        buildDiscarder(logRotator(artifactDaysToKeepStr: '1', artifactNumToKeepStr: '1', daysToKeepStr: '5', numToKeepStr: '10'))
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'obohovyk/node-jenkins:latest'
                    args '-u 0:0'
                }
            }
            environment {
                APP_NAME = "${env.GIT_URL.replaceFirst(/^.*\/([^\/]+?).git$/, '$1')}"
            }
            steps {
                sh('npm install')
                sh("tar cvzf build.tar.gz node_modules/")
                sh("echo ${env.APP_NAME}")

                stash includes: "build.tar.gz", name: 'artifact'
                archiveArtifacts artifacts: "build.tar.gz", fingerprint: true
            }
        }
    }
    post {
        success {
            sh "echo Pipeline completed successfully."
        }
        failure {
            echo "Build stage failed."
        }
    }
}
