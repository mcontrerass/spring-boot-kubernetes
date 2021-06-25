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

		stage ('SCA') {
            steps {
			    echo '========================================='
                echo '                SCA '
                echo '========================================='
                sh 'mvn org.owasp:dependency-check-maven:aggregate'
                dependencyCheckPublisher failedNewCritical: 5, failedTotalCritical: 10, pattern: '/var/jenkins_home/workspace/tareas_4_y_5_feature-SAST-SCA/target/target/dependency-check-report.xml', unstableNewCritical: 3, unstableTotalCritical: 5
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
						sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=feature-SAST-SCA -Dsonar.host.url=http://172.19.0.3:9000 -Dsonar.login=b431658c54f322c6f35296a32546f31d66813057 -Dsonar.sources=src/main/java/ -Dsonar.java.binaries=target/classes/"
                    }
                }
			}
        }
    }
}
