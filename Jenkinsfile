pipeline {
	agent any
	triggers {
		pollSCM('* * * * *')
	}
	stages {
		stage('Checkout'){
			agent{
				docker { image 'maven:3-openjdk-17' }
		}
			steps{
				git branch: 'main',
				url: 'https://github.com/kuhntat/source-maven-java-spring-hello-webapp.git'
			}
		}
		stage('Build'){
			agent{
				docker { image 'maven:3-openjdk-17' }
		}
			steps{
				sh 'mvn clean package'
			}
		}
		stage('Image Build'){
			agent{
				label 'controller'
		}
			steps{
				sh 'docker image build -t tomcat:hello .'
			}
		}
		stage('Deploy'){
			agent{
				label 'controller'
		}
			steps{
				sh 'docker container run -d -p 80:8080 --name webserver tomcat:hello'
			}
		}
	}
}
