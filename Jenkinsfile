pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
        MAVEN_OPTS = "-Xmx1024m -XX:MaxPermSize=256m"
    }

    stages {
        stage("Build") {
            steps {
                sh 'mvn clean install -Dsurefire.forkCount=1 -Dsurefire.reuseForks=false -DskipTests=false'
            }
        }
    }
}
