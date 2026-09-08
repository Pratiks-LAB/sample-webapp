@Library('jenkins-shared-repo') _

pipeline {
    agent {
		label 'workernode1'
	}

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: '77a9c587-4497-44d1-a3df-a96f89ff7d9c',
                    url: 'git@github.com:Pratiks-LAB/sample-webapp.git'
            }
        }

        stage('Build') {
            steps {
                buildApp()
            }
        }

        stage('Test') {
            steps {
                testApp()
            }
        }

        stage('Deploy') {
            steps {
				deployApp()
            }
        }
    }
}
