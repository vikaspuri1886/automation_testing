pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml clean install'
        }

      }
    }
    stage('Munit') {
      steps {
        sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml test'
      }
    }

   //stage('Deploy') {
     // steps {
     //  withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
      //    sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml package deploy -DmuleDeploy -Dtestfile=runner.TestRunner.java -Danypoint.username=joji4 -Danypoint.password=Canadavisa25@ -DapplicationName=apiops-bdd-sapi-jo -Dcloudhub.region=us-east-2'
     //  }


    //  }
   // }


    //stage('Checkout code') {
          //  steps {
            //     checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CheckoutOption', timeout: 60]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '6450a696-f924-4a1a-b70f-9e69524dcb53', url: 'https://github.com/jojivarghese25/Cucumber-automation.git']]])

           // }
        //}
    
   /* stage('SonarQube'){
            steps {
                withSonarQubeEnv('Sonarqube') {
                   sh "mvn -f sonarqube-poc/pom.xml sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.sources=src/"
           
                }
            }
        }
        stage('Quality Gate'){
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') { 
                        sh "curl -u admin:admin -X GET -H 'Accept: application/json' http://localhost:9000/api/qualitygates/project_status?projectKey=com.mycompany:sonarqube-poc > status.json"
                        def json = readJSON file:'status.json'
                        echo "${json.projectStatus}"
                        if ("${json.projectStatus.status}" != "OK") {
                            currentBuild.result = 'FAILURE'
                            error('Pipeline aborted due to quality gate failure.')
                        }
                    }
                }
            }*/

    stage('FunctionalTesting') {
     steps {
       withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f cucumber-API-Framework/pom.xml test'

       }

     }

       
       }

     

  stage('Reports') {
      steps {
        cucumber(fileIncludePattern: '**/cucumber.json', jsonReportDirectory: 'target')
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

}
