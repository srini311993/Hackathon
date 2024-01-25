pipeline {
environment {
registry = "srinisdockerepo"
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
    stage('SonarQube Analysis') {
      steps{
        script{
          def scannerHome = tool 'sonarscanner';
          withSonarQubeEnv() {
          sh "${scannerHome}/bin/sonar-scanner"
      }
      }
    }
  }
    stage('Quality Gate'){
      steps{
        script{
          timeout(time: 1, unit: 'MINUTES'){
          def qg = waitForQualityGate()
          if(qg.status != 'OK'){
              error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      } 
                }
        }
  }
    }
    stage('Docker Build'){
       steps {
         script {
            // def dockerHome = tool 'myDocker'
            // env.PATH = "${dockerHome}/bin:${env.PATH}"
            dockerImage = docker.build registry + "/nodejs:$BUILD_NUMBER"
            }
            }
      }
        stage('Scan Docker Image') {
          steps {
            sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL srinisdockerepo/nodejs:$BUILD_NUMBER'
          }
        }
     stage('Docker Push') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
          dockerImage.push()
    }
    }
    }
      }
    stage('Cleaning up') {
      steps{
        sh "docker rmi $registry/nodejs:$BUILD_NUMBER"
}
}
    }
    
      }
