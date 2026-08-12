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
                echo "Building WAR..."
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                echo "Testing..."
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                configFileProvider([
                    configFile(
                        fileId: '59eeda3c-2ee4-414f-aff8-d9c615118222',
                        variable: 'MAVEN_SETTINGS'
                    )
                ]) {
                    sh 'mvn clean deploy -s "$MAVEN_SETTINGS"'
                }
            }
        }
    }
}
