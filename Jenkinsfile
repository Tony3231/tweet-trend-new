pipeline {
     agent {label ''[Dmaven['}

environment {
     PATH = ""[D/opt/apache-maven-3.9.11/bin:$PATH"
}
     stages {
        stage("build"){
	steps {
	    sh 'mvn clean deploy'
	    }
	   }
	}

}
