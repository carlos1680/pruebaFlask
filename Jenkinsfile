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
				withCredentials([usernamePassword(credentialsId: 'MiDockerHub', passwordVariable: 'MiDockerHubPassword', usernameVariable: 'MiDockerHubUser')]) {
					sh "docker login -u ${env.MiDockerHubUser} -p ${env.MiDockerHubPassword}"
					sh 'docker push pirizito/prueba_plesk_1:latest'
				}
			}
		}
		stage('Actualizar contenedor'){
			agent any
			steps {
				sshagent(credentials: ['MiVpsLogin']) {
				  // sh "ssh -tt root@server.mechabios.com 'docker-compose -f /var/data/infraestructura/docker/fuentes/testing/ docker-compose-testing.yaml up -d --force-recreate --build flesk'"
				  sh "ssh -tt root@server.mechabios.com ls"
				}
			}
		}
	}
}