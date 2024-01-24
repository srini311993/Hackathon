pipeline {
    agent any
    stages {
      stage('clone repo') {
          steps {
            # Get the latest snapshot
            git clone https://github.com/srini311993/Hackathon.git myproject
    
            # Change directory
            cd myproject
          }
     stage('Build') {
        # Install NPM dependencies
        npm install
    }
         }
      }
    }
