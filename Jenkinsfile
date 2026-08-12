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

        stage('Deploy to Tomcat') {
            steps {
                configFileProvider([
                    configFile(
                        fileId: '59eeda3c-2ee4-414f-aff8-d9c615118222',
                        variable: 'MAVEN_SETTINGS'
                    )
                ]) {
                    sh '''
                        mvn -s $MAVEN_SETTINGS org.apache.maven.plugins:maven-dependency-plugin:3.7.0:copy \
                            -Dartifact=com.example:sample-webapp:1.5-SNAPSHOT:war \
                            -DoutputDirectory=/tmp \
                            # -DrepoUrl=http://artifactory.company.com/artifactory/lib-snapshot-local \
                            -Dtransitive=false

                        # After dependency:copy
                        LATEST_WAR=$(ls -t /tmp/sample-webapp-1.5-*.war | head -1)
                        echo "Latest WAR file: $(basename $LATEST_WAR)"

                        # Deploy to Tomcat
                        sudo cp $LATEST_WAR /opt/tomcat/tomcat-11/webapps/
                        sudo systemctl restart tomcat

                        echo "Tomcat restarted with latest artifact."
                    '''
                }
            }
        }
    }
}
