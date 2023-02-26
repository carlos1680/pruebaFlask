#!groovy
pipeline {
	agent none
	environment {
    remoteCommandsPlesk =
      """docker-compose -f :/var/data/infraestructura/docker/fuentes/testing/ docker-compose-testing.yaml up -d --force-recreate --build flesk """
  }
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
				withCredentials([usernamePassword(credentialsId: 'DockerPirizito', passwordVariable: 'DockerPirizitoPassword', usernameVariable: 'DockerPirizitoUser')]) {
					sh "docker login -u ${env.DockerPirizitoUser} -p ${env.DockerPirizitoPassword}"
					sh 'docker push pirizito/prueba_plesk_1:latest'
				}
			}
		}
		stage('Actualizar contenedor'){ 
			agent any
			steps {
				sshagent(credentials: ['SSHMechabios']) {
				  // sh "ssh -tt root@server.mechabios.com 'docker-compose -f /var/data/infraestructura/docker/fuentes/testing/docker-compose-testing.yaml up -d --force-recreate --build flesk'"
				  // Prueba webhook
				  //#!/bin/bash
				  //ssh -tt root@server.mechabios.com "cd /var/data/infraestructura/ && docker-compose up -d --do-deps --build python"
				  sh "ssh -i /var/jenkins_home/mechabios -tt root@server.mechabios.com 'cd /var/data/infraestructura/docker/fuentes/Testing/ && docker-compose -f docker-compose-testing.yaml up -d --no-deps --build flesk'"
				  //sh "ssh -i /var/jenkins_home/mechabios -tt root@server.mechabios.com 'ls'"
				}
			}
		}
	}
}