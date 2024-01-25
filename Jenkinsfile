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
    // stage("Trivy Scan") {
    //   steps {
    //     script {
    //       sh 'sudo curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sudo sh -s -- -b /usr/local/bin'
    //       sh 'trivy image --light --no-progress --format template --template "@//html.tpl" -o //workspace/$JOB_NAME/trivyscan.html dockerImage'
    //       publishHTML([ allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '//workspace/$JOB_NAME', reportFiles: 'trivyscan.html', reportName: 'TRIVY Scan Report', reportTitles: ''])
    //       }
    //       }
    //       }
        stage('Scan Docker Image') {
          steps {
            sh 'trivy dockerImage'
          }
        }
      //  stage('Vulnerability Scan - Docker Trivy') {
      //    steps {
      //      script{
      //        withCredentials([string(credentialsId: 'TRIVY_GITHUB_TOKEN', variable: 'TOKEN')]) {
      //        sh "sed -i 's#token_github#${TOKEN}#g' trivy-image-scan.sh"      
      //        sh "sudo bash trivy-image-scan.sh"
      //   }
      //  }
      // }
      //  }
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
