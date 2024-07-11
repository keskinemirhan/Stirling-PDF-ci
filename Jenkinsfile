pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
		sh 'chmod 755 gradlew'
                sh './gradlew build'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    def appVersion = sh(returnStdout: true, script: './gradlew printVersion -q').trim()
                    def image = "tsmonger/s-pdf:$appVersion"
                    sh "docker build -t $image ."
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    def appVersion = sh(returnStdout: true, script: './gradlew printVersion -q').trim()
                    def image = "tsmonger/s-pdf:$appVersion"
                    withCredentials([string(credentialsId: 'docker_hub_access_token', variable: 'DOCKER_HUB_ACCESS_TOKEN')]) {
                        sh "docker login --username tsmonger --password $DOCKER_HUB_ACCESS_TOKEN"
                        sh "docker push $image"
			sh "docker image rm $(docker image ls -q)"
                    }
                }
            }
        }*/
        stage('SSH Deploy') {
            steps {
                script {
	            def appVersion = sh(returnStdout: true, script: './gradlew printVersion -q').trim()
	            def image = "tsmonger/s-pdf:$appVersion"
	            withCredentials([string(credentialsId: 'ssh_remote_username', variable: 'SSH_USERNAME'),string(credentialsId: 'ssh_remote_addr',variable: 'SSH_ADDR')]) {
	                sshagent(credentials: ['ssh_remote_credentials']) {
	                    sh '''ssh $SSH_USERNAME@$SSH_ADDR bash -c "
				if [[ -z \"$(sudo docker image ls -q)\" ]] then
    				echo \"No Image Found\"
				else
    				sudo docker image rm $(sudo docker image ls -q)
				fi
    				if [[ -z \"$(sudo docker ps -q)\" ]] then
    				echo \"No Container Running\"
				else
    				sudo docker kill $(sudo docker container ps -q)
				fi
        			if [[ -z \"$(sudo docker ls -q)\" ]] then
    				echo \"No Container Found\"
				else
    				sudo docker container rm $(sudo docker container ls -q)
				fi
    				sudo docker run $image
				"'''
	                }
	            }
                }
		    
            }
        }
    }
}
