pipeline {
	environment {
		IMAGE_NAME = 'arunakilan/flask-demo:${env.BUILD_NUMBER}'
	}
	parameters {
	string(name: 'UNAME', defaultValue:'', description: 'enter your dockerhub username')
	password(name: 'PWD', defaultValue:'', description: 'enter your dockerhub password')
	}
	agent any
	stages {
		stage('git checkout') {
			steps {
				git 'https://github.com/Arun02136/flask-demo.git'
			}
		}
		stage('build image') {
			steps {
				sh 'docker build -t ${env.IMAGE_NAME} .'
			}
		}
		stage('login dockerhub') {
			steps {
		          sh "docker login -u ${param.UNAME} -p ${param.PWD}"
			}
		}
		stage('push image') {
			steps {
		          sh 'docker push ${env.IMAGE_NAME}'
			}
		}
		stage('Deploy to K8s') {
			steps {
				sshagent(['k8s-jenkins']) {
					sh 'scp -r -o StrictHostKeyChecking=no deployment.yaml arunakilan@ArunAkil:/home/python-demo'
					
					script {
						try {
						sh 'ssh arunakilan@ArunAkil kubectl apply -f /home/python-demo/deployment.yaml' //--kubeconfig=/path/kube.yaml'

						} catch(error) {

							}
					}
				}
			}
		}
	}
	post {
        	success { 
            		echo 'successfull push the docker image in dockerhub'
        	}

 }
}
