#!groovy
pipeline {
	agent none
	stages {
		stage('Docker Build') {
			agent any
			steps {
				sh 'docker build -t pirizito/prueba_plesk_1:latest .'
			}
		}
		stage('Docker Push') {
			agent any
			steps {
				withCredentials([usernamePassword(credentialsId: 'MiDockerHub', passwordVariable: 'MiDockerHubPassword', usernameVariable: 'MiDockerHubUser')]) {
					sh "docker login -u ${env.MiDockerHubUser} -p ${env.MiDockerHubPassword}"
					sh 'docker push pirizito/prueba_plesk_1:latest'
				}
			}
		}
	}
}