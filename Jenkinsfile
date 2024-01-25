pipeline {
environment {
registry = "https://hub.docker.com/repositories/srinisdockerepo"
registryCredential = 'dockerHub'
dockerImage = ''
}
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
    
    stage('Docker Build'){
       steps {
         script {
            def dockerHome = tool 'myDocker'
            env.PATH = "${dockerHome}/bin:${env.PATH}"
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
            }
    }
     stage('Docker Push') {
      agent any
      steps{
      script {
      docker.withRegistry( '', registryCredential ) {
      dockerImage.push()
    }
    }
    }
      }
    }
    
      }
