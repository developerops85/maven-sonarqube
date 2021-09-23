node() {

  tool name: 'maven', type: 'maven'
  
  stage ('Checkout')
  {
    cleanWs();
  git credentialsId: 'Kirtee-github-private-key', url: 'https://github.com/Kamthekirtee/maven-sonarqube.git'
  }
 
  stage ('Build')
  {
    withSonarQubeEnv(installationName: 'sonar'){
            sh 'mvn clean package' 
            //sh 'mvn clean verify sonar:sonar'
           }
          }
  
   try{
            stage("SonarQube Quality Gate") { 
                timeout(time: 1, unit: 'HOURS') { 
                def qg = waitForQualityGate() 
                if (qg.status != 'OK') {
                        echo "Quality Gate Failed"

                    }
                }
            }
            env.sonarqube_result = "PASS"
        }
  catch(Exception e){
            env.sonarqube_result = "FAIL"
            currentBuild.result = "FAILURE"
        }
  

}
