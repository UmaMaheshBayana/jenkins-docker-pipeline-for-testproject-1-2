# jenkins-docker-pipeline-for-testproject-1-2

1) Login to Jenkins and add docker username and password in Jenkins Credentials.
2) Create a declarative pipeline as per the script below.

```
pipeline {
	agent any
	environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-hud')
		}
	
	stages {
		stage('Fetch code') {
			steps {
				git branch: 'multistage-docker',url: 'https://github.com/UmaMaheshBayana/testproject-1-2.git'
				}
			}
		stage('build docker image') {
			steps {
				sh 'sudo docker build -t testproject-1-2:latest .'
				}
			}
		stage('Tagging docker image') {
		    steps {
		        sh 'sudo docker tag testproject-1-2 umamahesh7/testproject-1-2:latest'
		    }
		}
		stage('login') {
			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				}
			}
		stage('Push code') {
			steps {
				sh 'docker push umamahesh7/testproject-1-2:latest'
				}
			}
		}
		post {
			always {
				sh 'docker logout'
			}
		}
	}
  
  ````
