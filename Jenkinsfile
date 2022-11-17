pipeline {
agent any
stages {
	stage('Test') {
		steps {
            git branch:'main',url:'https://github.com/rachwrong/testing.git'
        }
    }

    stage('OWASP Check') {
      steps {
        dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
      }
    }

    stage('Code Quality Check via SonarQube') {
      steps {
        script {
          def scannerHome = tool 'SonarQube'; // <- Name of your SonarQube in Jenkins' System Configuration
            withSonarQubeEnv('SonarQube') { // <- Same as above?
              sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=test -Dsonar.sources=." // <- Project key should be the name of your project on SonarQube's webpage
            }
        }
      }
    }
  }
post {
    always {
        dependencyCheckPublisher pattern: 'dependency-check-report.xml'
    }
    always {
        recordIssues enabledForFailure: true, tool: sonarQube()
    }
    }
}