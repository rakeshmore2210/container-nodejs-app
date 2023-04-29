pipeline {
    agent any

    stages {
        stage('GIT CHECKOUT') {
            steps {
                echo 'GIT CHECKOUT'
				checkout([$class: 'GitSCM', branches: [[name: '*/master']],
				userRemoteConfigs: [[url: 'https://github.com/rakeshmore2210/container-nodejs-app.git']]])
            }
        }
        stage('DOCKER BUILD') {
            steps {
                echo 'DOCKER BUILD'
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 228426373888.dkr.ecr.us-east-1.amazonaws.com'
				sh 'docker build -t 228426373888.dkr.ecr.us-east-1.amazonaws.com/upgrad_rakesh_ecr_nodejs_app:v1 .'
				sh 'docker push 228426373888.dkr.ecr.us-east-1.amazonaws.com/upgrad_rakesh_ecr_nodejs_app:v1'
            }
        }
        stage('CONTAINER DEPLOYMENT') {
			agent APP_SERVER
            steps {
                echo 'CONTAINER DEPLOYMENT'
				sh '''
					if [ "$(docker ps -a -q -f name=nodejsapp)" ]; then
						docker rm -f nodejsapp
					fi
					docker run -d --name nodejsapp 228426373888.dkr.ecr.us-east-1.amazonaws.com/upgrad_rakesh_ecr_nodejs_app:v1
					'''
            }
        }
    }
}
