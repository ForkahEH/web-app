pipeline{
  agent any
  tools{
      maven "maven3.8.8"
  }
   stages{
  stage("1. Build from Maven"){
    steps{
     sh "echo start git clone from Repository"
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
  
  stage("3. Code Scan with Sonarqube"){
    steps{
        sh "echo start of code scan using sonarqube"
        sh "mvn sonar:sonar"
        sh "echo end of code scan using sonarqube"
    }
  }
  
  stage("4. Deploy artifacts to Nexus"){
     steps{
         sh "echo Deploy to Nexus"
         sh "mvn deploy"
         sh "echo end Deploy to Nexus"
     }
  }
  
  stage("5. Deploy to Tomcat Server in UAT"){
    steps{
        sh "echo start deploying to tomcat server in UAT Environment"
        deploy adapters: [tomcat9(credentialsId: 'tomcard_credtial', path: '', url: 'http://18.118.1.16:9090/')], contextPath: null, war: 'target/*.war'
        sh "echo end deploying to tomcat server in UAT Environment"
    }
  }
  
  stage("6. Notification Alert"){
    steps{
        sh " echo start email notification to DevOps Team"
        emailext body: 'Declarative Project - Email Alert', subject: 'Declarative Project', to: 'info@jomacsit.com'
        sh " echo end email notification to DevOps Team"
    }
  }
  
 }
}
