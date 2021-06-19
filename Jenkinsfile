pipeline {
	agent any

	stages {
		stage('Build') {
			steps {
				echo 'Building...'
			}
		}
	
		stage('SAST') {
                        steps {
                                echo 'Pruebas de analisis estatico'
                        }
                }

		stage('Test') {
			steps {
				echo 'Testing...'
			}
		}

		stage('Deploy') {
                        steps {
                                echo 'Deploying...'
                        }
                }
	}
}
