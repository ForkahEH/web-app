pipeline{
  agent any
  tools{
    maven "Maven3.8.8"
  }
        stages{
          stage("1. Git clone from repo"){
            steps{
              sh "echo start of git clone"
              git branch: 'main', url: 'https://github.com/JOMACS-IT/web-app-Annex.git'
              sh "echo end of git clone"
            }
          }
          
          stage("2. Build from Maven"){
            steps{
              sh "echo start building from Maven"
              sh "mvn clean package"
              sh "echo end of build"
            }
          }
          
          stage("3. Code Scan"){
            steps{
              sh "echo start of code scan"
              sh "mvn sonar:sonar"
              sh "echo end of code scan"
            }
          }
        
          stage("4. Store Artifacts"){
            steps{
              sh "echo start depolying to Nexus"
              sh "mvn deploy"
              sh "echo end of Delpoying "
            }
          }
          
          stage("5. Delpoying to Tomcat in UAT"){
            steps{
              sh "echo start deploying to server in UAT Env."
              deploy adapters: [tomcat9(credentialsId: 'tomcard_cred.', path: '', url: 'http://http://13.49.244.253/:9090/')], contextPath: null, war: 'target/*.war'
            }
          }
          
          stage("6. Send E-mail Notifcation"){
            steps{
              sh "echo E-mail Notification to DevOps Team"
              emailext body: 'The Deployment is Successful - Email Alert.', subject: 'Deployment Succes - Email Notification', to: 'ben.p.tawiah@gmail.com'
            }
          }
        }
}
