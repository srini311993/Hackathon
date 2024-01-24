node(){
    deleteDir()
        try{
            stage('scm checkout from github'){
                git branch: 'master', credentialsId: '66ffcb17-2356-49a4-82d6-30ba8b18471c', url: 'https://atomstate@dev.azure.com/atomstate/CartonBackend/_git/CartonBackend'
            }
            stage('Build'){
                sh "mvn clean package"
            }
            stage('junit & jacoco'){
                //junit '**/target/surefire-reports/*.xml'
                junit allowEmptyResults: true, testResults: '**/test-results/*.xml'
                jacoco()
            }
            stage('checkstyle'){
                sh "mvn checkstyle:check"
                recordIssues(tools: [checkStyle(reportEncoding: 'UTF-8')])
            }
            stage('sonar'){
                withSonarQubeEnv(credentialsId: 'sonar-token2'){
                    sh "mvn sonar:sonar -f pom.xml -Dsonar.host.url=http://65.0.104.142:9000 -Dsonar.login=92744b803a5c17a230e889aecc5c09bd3507f77f -Dsonar.projectKey=poc"
                }

            }
            stage('quality gate'){
                timeout(time: 1, unit: 'HOURS'){
                    def qg = waitForQualityGate()
                    if(qg.status != 'OK'){
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                    
                }
            }
            stage('Approval'){
                input id: "${env.BUILD_ID}", message: 'continute to post build '
            }
            stage('Docker image'){
                //sh "docker rmi ${docker images | grep dockerpoc | tr -s ' ' | cut -d ' ' -f 3}"
                sh "docker build -t 13.126.195.126:8085/dockerpoc/dambackend:${env.BUILD_ID} ."
            }
            stage('Pushing Docker image to Nexus'){
                withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-token', vaultUrl: 'http://13.126.195.126:8200'], vaultSecrets: [[path: 'secrets/nexus', secretValues: [[vaultKey: 'username'], [vaultKey: 'password']]]]){
                    sh "docker login -u '${username}' -p '${password}' http://13.126.195.126:8085"
                }
                sh "docker push 13.126.195.126:8085/dockerpoc/dambackend:${env.BUILD_ID}" 

            }

        }catch(err){
            println("================ ERROR: ${err}")
            currentBuild.result = "FAILURE"
            error()   
        }
}
