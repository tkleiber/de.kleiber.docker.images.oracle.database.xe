pipeline {
  agent {
    node {
      label 'localhost_vagrant'
    }
    
  }
  stages {
    stage('Start Oracle XE Container') {
      steps {
        sh '''echo start Oracle XE container
sudo docker run --name=db11.2.0.2-xe --shm-size=1g --restart=unless-stopped -p 1521:1521 -p 80:8080 -p 5500:5500 -e ORACLE_PWD=oracle localhost:5000/oracle/database:11.2.0.2-xe &
echo wait for successful start
wget http://localhost/apex --retry-connrefused --tries=0 -q --wait=3 --spider --connect-timeout=60
echo show container
sudo docker ps'''
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
        sh '''docker stop db11.2.0.2-xe
docker rm db11.2.0.2-xe'''
      }
    }
  }
}