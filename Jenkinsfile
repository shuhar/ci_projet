pipeline {
    agent any

    stages {
		stage('Clone') {
		steps {
			script {
				git 'https://github.com/shuhar/ci_projet.git';
				echo 'Clone done with success!'
				}
				}
				}
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
