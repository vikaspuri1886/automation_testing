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
          sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml package deploy -DmuleDeploy -Dtestfile=runner.TestRunner.java -Danypoint.username=joji4 -Danypoint.password=Canadavisa25@ -DapplicationName=apiops-bdd-sapi-joji1 -Dcloudhub.region=us-east-2'
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
              sh 'mvn -f Cucumber-automation/cucumber-API-Framework/pom.xml test -Dtestfile=cucumber-API-Framework.java'
            }
        }

   // stage('FunctionalTesting') {
   //   steps {
    //    withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
      //    sh 'mvn -f Cucumber-automation/cucumber-API-Framework/pom.xml test -Dtestfile=cucumber-API-Framework.java'
      //  }

    //  }
   // }



    //stage('Email') {
     // steps {
     //   emailext(subject: 'Testing Reports for $PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'Please find the functional testing reports. In order to check the logs also, please go to url: $BUILD_URL'+readFile("apiops-anypoint-bdd-sapi/emailTemplate.html"), attachmentsPattern: 'apiops-anypoint-bdd-sapi/target/cucumber-reports/report.html', from: "jojisham13@gmail.com", mimeType: "text/html", to: "jojihr@gmail.com")
     // }
  //  }

 // }
  tools {
    maven 'Maven'
  }
  post {
    failure {
      emailext(subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'Please find attached logs.', attachLog: true, from: "jojisham13@gmail.com", to: "jojihr@gmail.com")
    }

  }
}
