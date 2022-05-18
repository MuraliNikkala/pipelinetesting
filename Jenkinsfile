pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf pipelinetesting_master'
            sh 'rm -rf pipelinetesting'
            sh 'git clone https://github.com/MuraliNikkala/pipelinetesting.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipelinetesting_master'
            sh 'docker build -t muralinikkala/pipelinetestmaster:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push muralinikkala/pipelinetestmaster:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.100:2375 stop masterwebapp1 || true'
            sh    'docker -H tcp://10.1.1.100:2375 run --rm -dit --name masterwebapp1 --hostname masterwebapp1 -p 8000:80 muralinikkala/pipelinetestmaster:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.100:8000'
          }
        }

    }
}

