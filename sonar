pipeline {
  agent any
  stages {
    stage ('Checkout') {
      steps {
        git branch:'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
      }
    }
    stage('Code Quality Check via SonarQube') {
      steps {
        script {
          def scannerHome = tool 'SonarQube'; // <- Name of your SonarQube in Jenkins' System Configuration
            withSonarQubeEnv('SonarQube') { // <- Same as above?
              sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=." // <- Project key should be the name of your project on SonarQube's webpage
            }
        }
      }
    }
  }
  post {
    always {
      recordIssues enabledForFailure: true, tool: sonarQube()
    }
  }
}
