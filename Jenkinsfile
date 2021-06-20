pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
      stage ('Initial') {
            steps {
              sh '''
                   echo "PATH = ${PATH}"
                   echo "M2_HOME = ${M2_HOME}"
               '''
            }
        }
        stage ('Compile') {
            steps {
              sh 'mvn clean compile -e'
            }
        }
        stage ('Test') {
            steps {
              sh 'mvn clean test -e'
            }
        }

        stage ('Jar') {
            steps {
              sh 'mvn clean package'
            }
        }

        stage('SAST') {
			steps{
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
