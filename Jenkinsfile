pipeline {
  agent {
    node {
      label 'localhost_vagrant'
    }
    
  }
  stages {
    stage('Start Oracle XE Container') {
      steps {
        parallel(
          "Start Oracle XE Container": {
            echo 'Start'
            
          },
          "Wait for Oracle Container Start": {
            echo 'Wait'
            
          }
        )
      }
    }
    stage('Configure XE Container') {
      steps {
        echo 'Configure'
      }
    }
    stage('Push XE Container to Registry') {
      steps {
        echo 'Push'
      }
    }
    stage('Clean Up') {
      steps {
        echo 'Clean Up'
      }
    }
  }
}