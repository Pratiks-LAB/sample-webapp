pipeline {

    agent any

    stages {

        stage('Checkout') {
            agent {
                retries 2
            }
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

        stage('Deploy to Tomcat') {
            steps {
                echo "Downloading WAR from Artifactory..."

                sh '''
                    rm -f /opt/tomcat/tomcat-10/webapps/*.war

                    wget -O /opt/tomcat/tomcat-10/webapps/sample-webapp.war \
                    http://54.208.43.126:8081/artifactory/libs-snapshot-local/com/example/sample-webapp/1.0-SNAPSHOT/sample-webapp-1.0-SNAPSHOT.war
                '''

                echo "WAR deployed to Tomcat webapps."
            }
        }
    }
}
