pipeline {
  agent any
  stages {
    stage('Check SonarScanner Installation') {
      steps {
        script {
          def scannerHome = tool 'MySonarScanner'
          echo "SonarScanner should be installed at: ${scannerHome}"
          withEnv(["JAVA_HOME=C:\\Program Files\\Java\\jdk-21"]) {
            bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -v"
          }
        }
      }
    }
    stage('Checkout GitHub') {
      steps {
        script {
          def branchName = env.BRANCH_NAME ?: 'main'
          echo "Branche détectée : ${branchName}"
          checkout([
            $class: 'GitSCM',
            branches: [[name: "*/${branchName}"]],
            userRemoteConfigs: [[
              url: 'https://github.com/hbaiebmaki-pixel/atelier-github.git'
            ]]
          ])
        }
      }
    }
    stage('Analyse SonarQube') {
      steps {
        script {
          def scannerHome = tool 'MySonarScanner'
          withEnv(["JAVA_HOME=C:\\Program Files\\Java\\jdk-21"]) {
            withSonarQubeEnv('Project') {
              withCredentials([string(credentialsId: 'sonarqube', variable: 'TOKEN')]) {
                bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" " +
                  "-Dsonar.projectKey=node-projetcid " +
                  "-Dsonar.sources=. " +
                  "-Dsonar.login=%TOKEN% " +
                  "-Dsonar.projectVersion=1.0.0 " +
                  "-Dsonar.sourceEncoding=UTF-8"
              }
            }
          }
        }
      }
    }
  }
}
