pipeline {
    agent { label 'maven-slave' }

    environment {
        // SonarQube credentials from Jenkins
        SONAR_TOKEN = credentials('sonar-credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout from GitHub
                git branch: 'main',
                    url: 'https://github.com/Tony3231/tweet-trend-new.git',
                    credentialsId: 'Github_credentials'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project with Maven...'
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                sh """
                    mvn sonar:sonar \
                    -Dsonar.projectKey=tweet-trend \
                    -Dsonar.organization=namg04-key \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.login=${SONAR_TOKEN}
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Build or SonarQube analysis failed!'
        }
    }
}
