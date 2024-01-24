node(){
    deleteDir()
        tools {nodejs "node"}
        try{
            stage('scm checkout from github'){
                git branch: 'master', url: 'https://github.com/srini311993/Hackathon.git'
            }
            stage('Build'){
                sh "npm install"
            }

        }catch(err){
            println("================ ERROR: ${err}")
            currentBuild.result = "FAILURE"
            error()   
        }
}
