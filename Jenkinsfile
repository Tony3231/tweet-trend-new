pipeline {
    agent any

    tools {
    jdk 'jdk17'            // must match your JDK installation name
    maven 'Maven 3.9.6'    // must match Maven installation name
}


    environment {
        SONAR_TOKEN = credentials('sonar-token-id')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build and Sonar analysis succeeded.'
        }
        failure {
            echo 'Build or Sonar analysis failed.'
        }
    }
}
