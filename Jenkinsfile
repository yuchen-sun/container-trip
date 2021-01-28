pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''git describe --tags `git rev-list --tags --max-count=1`
env'''
      }
    }

  }
}