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
            sh '''echo start Oracle XE container in Background because of the log output
sudo docker run --name=db11.2.0.2-xe --shm-size=1g --restart=unless-stopped -p 1521:1521 -p 80:8080 -p 5500:5500 -e ORACLE_PWD=oracle localhost:5000/oracle/database:11.2.0.2-xe &'''
            
          },
          "Wait for successful start": {
            sh '''echo wait for successful start
wget http://localhost/apex --retry-connrefused --tries=0 -q --wait=3 --spider --connect-timeout=60
echo show container
sudo docker ps'''
            
          }
        )
      }
    }
    stage('Install utPLSQL') {
      steps {
        echo 'Configure'
        sh '''# Download the latest release "zip" file
sudo docker exec db11.2.0.2-xe curl -LOk $(curl --silent https://api.github.com/repos/utPLSQL/utPLSQL/releases/latest | awk '/browser_download_url/ { print $2 }' | grep ".zip" | sed 's/"//g') 
# Extract downloaded "zip" file
sudo docker exec db11.2.0.2-xe unzip -q utPLSQL.zip
# Headless installation
sudo docker exec db11.2.0.2-xe sqlplus sys/oracle@//localhost:1521/XE as sysdba @utPLSQL/source/install_headless.sql '''
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