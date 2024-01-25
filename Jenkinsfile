pipeline {
  agent any
  tools {nodejs 'nodejs'}
  stages {
        
    stage('Git') {
      steps {
        git 'https://github.com/srini311993/Hackathon.git'
      }
    }
     
    stage('Build') {
      steps {
        sh 'npm install -g nodemon'
      }
    }
    stage('Docker image'){
    tools {Docker 'latest'}
       steps {
        sh 'docker build -t node/nodejs:latest .'
            }
            }
      }
}
