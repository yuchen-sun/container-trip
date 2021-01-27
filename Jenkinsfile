pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''docker build \\
      -t 192.168.100.12:5000/wiki-trip:latest .'''
      }
    }

  }
}