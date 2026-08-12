pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/Pratiks-LAB/sample-webapp.git'
                )
            }
        }

        stage('Build') {
            steps {
                echo "Bulding war ... "
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                echo "testing"
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo "deploying to artifact.. "
                configFileProvider([
                    configFile(
                        fileId: '59eeda3c-2ee4-414f-aff8-d9c615118222',
                        variable: 'MySettings'
                    )
                ]) {
                    sh 'mvn deploy -s "$MySettings"'
                }
            }
        }
    }
}