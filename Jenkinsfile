pipeline {
    agent { label 'maven-slave' }

    environment {
        SONAR_TOKEN = credentials('sonar-credentials')
        GIT_CREDENTIALS = 'Github_credentials'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Checking out Git repository...'
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Tony3231/tweet-trend-new.git',
                        credentialsId: "${GIT_CREDENTIALS}"
                    ]]
                ])
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project with Maven...'
                script {
                    // Use the Maven tool configured in Jenkins
                    def mvnHome = tool name: 'Maven3', type: 'maven'
                    sh "${mvnHome}/bin/mvn clean install"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                script {
                    def mvnHome = tool name: 'Maven3', type: 'maven'
                    sh """
                        ${mvnHome}/bin/mvn sonar:sonar \
                        -Dsonar.projectKey=tweet-trend-new \
                        -Dsonar.host.url=http://your-sonarqube-server:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube analysis completed successfully!'
        }
        failure {
            echo 'Build or SonarQube analysis failed!'
        }
        always {
            cleanWs()
        }
    }
}
