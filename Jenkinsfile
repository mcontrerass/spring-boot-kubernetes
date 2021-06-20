pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
      stage ('Initial') {
            steps {
              echo '========================================='
              echo '                INITIAL '
              echo '========================================='
              sh '''
                   echo "PATH = ${PATH}"
                   echo "M2_HOME = ${M2_HOME}"
               '''
            }
        }
        stage ('Compile') {
            steps {
                echo '========================================='
                echo '                COMPILE '
                echo '========================================='
                 sh 'mvn clean compile -e'
            }
        }
        stage ('Test') {
            steps {
                echo '========================================='
                echo '                TEST '
                echo '========================================='
                 sh 'mvn clean test -e'
            }
        }

        stage('SonarQube analysis') {
           steps{
               echo '========================================='
              echo '                SONARQUBE '
              echo '========================================='
                script {
                    def scannerHome = tool 'SonarQube Scanner';//def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('Sonar Server') {
					echo "ScannerHome: ${scannerHome}"
					bat "${scannerHome}\\bin\\sonar-scanner -Dsonar.projectKey=feature-SAST -Dsonar.sources=target/ -Dsonar.host.url=http://localhost:9000 -Dsonar.login=ccc1e1bac97713224def5efdf37c33702512a9c0"
                    }
                }
           }
        }
    }
}
