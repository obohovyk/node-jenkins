pipeline {
    agent {
        docker { 
            image 'obohovyk/node-jenkins:latest'
            args '-u jenkins:jenkins'
        }
    }
    stages {
        stage('Node version') {
            steps {
                sh 'node --version'
                sh 'npm i'
            }
        }
    }
    post{
        success {
            sh "echo 'Pipeline completed successfully.'"
        }
        failure{
            sh "echo 'Pipeline failed.'"
        }
    }
}