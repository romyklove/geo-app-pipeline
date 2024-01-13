pipeline {
    agent any

    tools {
         maven 'M2_HOME'
        jfrog 'Jfrog remote cli'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/romyklove/geo-app-pipeline.git'
            }
        }
        stage('Sonarqube scan') {
            steps {
                withSonarQubeEnv('sonarQube') {
                    sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=romyklove_geo-app-pipeline'
                }
            }
        }

        stage('Testing2') {
            steps {
                // Show the installed version of JFrog CLI.
                jf '-v'
                // Show the configured JFrog Platform instances.
                jf 'c show'
                // Ping Artifactory.
                jf 'rt ping'
                // Create a file and upload it to a repository named 'geoapp' in Artifactory
                sh 'touch geo-app'
                jf 'rt u geo-app geoapp/'
                // Publish the build-info to Artifactory.
                jf 'rt bp'
                // Download the test-file
                jf 'rt dl geoapp/geo-app'
            }
        }

    }
}

