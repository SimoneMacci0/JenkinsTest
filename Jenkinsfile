pipeline {
	agent none  

    tools {
		jdk 'Java17'
	        docker 'docker'
	}

    environment {
        pwd = credentials('DOCKER_HUB_PWD')
    }

    stages {
  	stage('Maven Install') {
    	agent {
      	docker {
        	image 'maven:3.6.3'
        }
      }
      steps {
      	sh 'mvn clean package'
      }
    }
    stage('Docker Build') {
    	agent any
      steps {
      	sh 'docker build -t simomaccio/jenkins-test:latest .'
      }
    }
    stage('Docker Push') {
    	agent any
      steps {
      	withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: pwd, usernameVariable: 'simomaccio')]) {
        	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push simomaccio/jenkins-test:latest'
        }
      }
    }
  }
}
