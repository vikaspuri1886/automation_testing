pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml clean install -Dtestfile=runner.TestRunner.java -DskipTests'
        }

      }
    }

    stage('Deploy') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml package deploy -DmuleDeploy -Dtestfile=runner.TestRunner.java -Danypoint.username=joji4 -Danypoint.password=Canadavisa25@ -DapplicationName=apiops-bdd-sapi-jo -Dcloudhub.region=us-east-2'
        }

      }
    }
   
    //stage('Clone another repository') {
    //  steps {
     //   git branch: 'master',
     //   credentialsId: '87af46ec-e10a-46f3-accd-60bfacb10526',
     //   url: 'https://github.com/jojivarghese25/Cucumber-automation.git'
     // }
   // }
    stage('Checkout code') {
            steps {
                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CheckoutOption', timeout: 60]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '6450a696-f924-4a1a-b70f-9e69524dcb53', url: 'https://github.com/jojivarghese25/Cucumber-automation.git']]])
              
            }
        }

    stage('FunctionalTesting') {
     steps {
       withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f cucumber-API-Framework/pom.xml test' -Dcucumber.options="src/test/java/features/ --tags ~@Ignore"}
       
     
     }
    }

   stage('Cucumber Reports') {
           steps {
                 cucumber buildStatus: "UNSTABLE",
                   fileIncludePattern: "**/cucumber.json",
                   jsonReportDirectory: 'target'
              
            }
        }


    stage('Email') {
      steps {
        emailext attachmentsPattern: 'cucumber-API-Framework/target/cucumber-reports/report.html', body: 'please find attached report', replyTo: 'jojisham13@gmail.com', subject: 'Reports', to: 'jojisham13@gmail.com'
      }
    }

  }
  tools {
    maven 'Maven'
  }
  //post {
  //  failure {
   //   emailext(subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'Please find attached logs.', attachLog: true, from: "jojisham13@gmail.com", to: "jojisham13@gmail.com")
   // }

 // }
}

