pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''docker buildx build \\
      --platform linux/amd64,linux/arm64 \\
      -t docker/getting-started:latest \\
      $( (( $WILL_PUSH == 1 )) && printf %s \'--push\' ) .'''
      }
    }

  }
}