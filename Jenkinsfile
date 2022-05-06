pipeline {
	agent any
	environment {
		PATH= "/opt/apache-maven-3.8.5/bin:$PATH"
        //* in the jenkins server run $whereis mvn
	}
	stages {
		stage('git'){
			steps{
				git 'https://github.com/ravdy/hello-world.git'
			}
		}
		stage('Maven-Build'){
			steps {
				sh 'mvn clean install'
			}
		}
		stage('copy the war file'){
			steps {
				sshagent(['some-random-number']) {
				sh 'scp -v -o StrictHostKeyChecking=no webapp/target/webapp.war user@xx.xx.xx.xx:~/webapp'
}
			}
		}
		stage('Docker build'){
			steps {
				sshagent(['d8b6b8b2-dfac-4ad7-9862-0be13c434f2e']) {
					sh '''
						ssh -o StrictHostKeyChecking=no user@xx.xx.xx.xx <<EOF
						cd ~/webapp
						ls -al
						docker build -t mywebapp .
						docker run -p 8080:8080 mywebapp
					'''
				}
			}
		}
	}
}