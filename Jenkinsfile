pipeline {
  agent {
    node {
      label 'localhost_vagrant'
    }
    
  }
  stages {
    stage('Get Oracle Docker Sources') {
      steps {
        echo 'Test'
        git(url: 'https://github.com/oracle/docker-images.git', changelog: true, poll: true, credentialsId: '	1abca346-d215-4d44-8adf-64f2264e9614')
      }
    }
  }
}