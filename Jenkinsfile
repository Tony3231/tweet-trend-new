pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }

    stages {
        stage("Build") {
            steps {
                // Limit surefire forks to prevent VM crash
                sh 'mvn clean deploy -Dsurefire.forkCount=1 -Dsurefire.reuseForks=false'
            }
        }
    }
}
