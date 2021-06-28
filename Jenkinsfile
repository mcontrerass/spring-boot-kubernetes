pipeline {
    agent any

    tools {
        maven 'Maven'
		nodejs "NodeJS"
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
                dependencyCheckPublisher failedNewCritical: 5, failedTotalCritical: 10, pattern: '**/dependency-check-report.xml', unstableNewCritical: 3, unstableTotalCritical: 5
            }
        }
		
		stage('SAST') {
			steps {
			    echo '========================================='
                echo '                SAST '
                echo '========================================='
				script {
                    def scannerHome = tool 'SonarQube Scanner';//def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('Sonar Server') {
						echo "ScannerHome: ${scannerHome}"
						sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=feature-SAST-SCA -Dsonar.host.url=http://172.19.0.3:9000 -Dsonar.login=b431658c54f322c6f35296a32546f31d66813057 -Dsonar.sources=src/main/java/ -Dsonar.java.binaries=target/classes/ -Dsonar.dependencyCheck.xmlReportPath=target/dependency-check-report.xml -Dsonar.dependencyCheck.jsonReportPath=target/dependency-check-report.json -Dsonar.dependencyCheck.htmlReportPath=target/dependency-check-report.html"
                    }
                }
			}
        }
		
		stage('ZAP') {
        	steps {
        	    figlet 'Owasp Zap DAST'
        		script {
        		    env.DOCKER = tool 'Docker';
					env.DOCKER_EXEC = "${DOCKER}/bin/docker"
					echo "${DOCKER_EXEC}"
        		    //sh '${DOCKER_EXEC} rm -f zap2'
        		    sh '${DOCKER_EXEC} pull owasp/zap2docker-stable'
                    sh '${DOCKER_EXEC} run --add-host="localhost:192.168.56.201" --rm -e LC_ALL=C.UTF-8 -e LANG=C.UTF-8 --name zap2 -u zap -p 8090:8080 -d owasp/zap2docker-stable zap.sh -daemon -port 8080 -host 0.0.0.0 -config api.disablekey=true'
                    sh '${DOCKER_EXEC} run --add-host="localhost:192.168.56.201" -v /home/vagrant/owasp-zap:/zap/wrk/:rw --rm -i owasp/zap2docker-stable zap-full-scan.py -t "http://zero.webappsecurity.com" -I -r zap_full_scan_report2.html -l PASS'
        		}
        	}
        }
	}
}
