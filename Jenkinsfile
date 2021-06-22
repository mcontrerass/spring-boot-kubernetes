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

        stage ('Jar') {
            steps {
			  echo '========================================='
              echo '                JAR '
              echo '========================================='
              sh 'mvn clean package'
            }
        }

        stage('SAST') {
			steps{
			    echo '========================================='
                echo '                SAST '
                echo '========================================='
				script {
                    def scannerHome = tool 'SonarQube Scanner';//def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('Sonar Server') {
						echo "ScannerHome: ${scannerHome}"
						bat "${scannerHome}\\bin\\sonar-scanner mvn sonar:sonar -Dsonar.projectKey=feature-SAST-SCA -Dsonar.sources=target/ -Dsonar.host.url=http://localhost:9000 -Dsonar.login=9e200297885d831331f989b01ae0079411577b2d"
                    }
                }
			}
        }
		
		stage ('SCA') {
            steps {
			    echo '========================================='
                echo '                SCA '
                echo '========================================='
                bat 'mvn org.owasp:dependency-check-maven:check'
                dependencyCheckPublisher failedNewCritical: 5, failedTotalCritical: 10, pattern: 'target/dad.xml', unstableNewCritical: 3, unstableTotalCritical: 5
            }
        }
    }
}
