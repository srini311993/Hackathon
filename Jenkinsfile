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
    
    stage('Docker Build'){
       steps {
         script {
            def dockerHome = tool 'myDocker'
            env.PATH = "${dockerHome}/bin:${env.PATH}"
            sh 'docker build -t srinisdockerepo/nodejs:${env.BUILD_ID} .'
            }
            }
    }
     stage('Docker Push') {
    	agent any
      steps {
      	withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push srinisdockerepo/nodejs:${env.BUILD_ID}'
        }
      }
    }
    
      }
}
