pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo build'
            }
        }
        stage('Deploy') {
            when { tag "v*" }
            steps {
                echo 'Deploying only because this commit is tagged...'
                sh 'env'
            }
        }
    }
}
