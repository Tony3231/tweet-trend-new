pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.11/bin:$PATH"
    }

    stages {
        stage("Clone") {
            steps {
                git branch: 'main', url: 'https://github.com/Tony3231/tweet-trend-new'
            }
        }

        stage("Build") {
            steps {
                sh 'mvn clean deploy'
            }
        }
    }
}
