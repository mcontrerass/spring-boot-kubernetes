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
		
		stage('ZAP') {
        	steps {
        	    figlet 'Owasp Zap DAST'
        		script {
        		    env.DOCKER = tool 'Docker';
					env.DOCKER_EXEC = "${DOCKER}/bin/docker"
					echo "${DOCKER_EXEC}"
        		    sh '${DOCKER_EXEC} rm -f zap2'
        		    sh '${DOCKER_EXEC} pull owasp/zap2docker-stable'
                    sh '${DOCKER_EXEC} run --add-host="localhost:192.168.56.201" --rm -e LC_ALL=C.UTF-8 -e LANG=C.UTF-8 --name zap2 -u zap -p 8090:8080 -d owasp/zap2docker-stable zap.sh -daemon -port 8080 -host 0.0.0.0 -config api.disablekey=true'
                    sh '${DOCKER_EXEC} run --add-host="localhost:192.168.56.201" -v /home/vagrant/owasp-zap:/zap/wrk/:rw --rm -i owasp/zap2docker-stable zap-full-scan.py -t "http://zero.webappsecurity.com" -I -r zap_full_scan_report2.html -l PASS'
        		}
        	}
        }
	}
}
	
