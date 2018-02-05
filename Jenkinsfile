pipeline {
  agent {
    node {
      label 'localhost_vagrant'
    }

  }
  stages {
    stage('Create Oracle XE Container') {
      steps {
        parallel(
          "Start Oracle XE Container": {
            sh '''echo delete persistent files
rm -r /var/lib/oracle/database/11.2.0.2/xe
mkdir /var/lib/oracle/database/11.2.0.2/xe
echo start Oracle XE container in Background because of the log output
docker run --name=db11.2.0.2-xe --shm-size=1g --restart=unless-stopped -p 1521:1521 -p 80:8080 -p 5500:5500 -e ORACLE_PWD=oracle -v /var/lib/oracle/database/11.2.0.2/xe:/u01/app/oracle/oradata localhost:5000/oracle/database:11.2.0.2-xe &'''

          },
          "Wait for successful start of Oracle XE Container": {
            sh '''echo wait for successful start
wget http://localhost/apex --retry-connrefused --tries=0 -q --wait=3 --spider --connect-timeout=60
echo show container
docker ps'''

          }
        )
      }
    }
    stage('Install utPLSQL') {
      steps {
        echo 'Configure'
        sh '''# Download the latest release "zip" file
docker exec db11.2.0.2-xe curl -LOk $(curl --silent https://api.github.com/repos/utPLSQL/utPLSQL/releases/latest | awk '/browser_download_url/ { print $2 }' | grep ".zip" | sed 's/"//g')
# Extract downloaded "zip" file
docker exec db11.2.0.2-xe unzip -q utPLSQL.zip
# Headless installation
docker exec db11.2.0.2-xe bash -c 'cd utPLSQL/source; sqlplus sys/oracle@//localhost:1521/XE as sysdba @install_headless.sql' '''
      }
    }
    stage('Commit Oracle XE Container to Image and push it') {
      steps {
        sh '''docker commit -c "EXPOSE 1521 8080 5500" db11.2.0.2-xe localhost:5000/tkleiber/database:11.2.0.2-xe
docker push localhost:5000/tkleiber/database:11.2.0.2-xe'''
      }
    }
    stage('Clean Up Oracle XE Container') {
      steps {
        sh '''docker stop db11.2.0.2-xe
docker rm db11.2.0.2-xe'''
      }
    }
  }
}
