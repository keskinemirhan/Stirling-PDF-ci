pipeline {
    agent any
    stages {
      /*  stage('Build') {
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
                    }
                }
            }
        }*/
        stage('SSH Deploy') {
            steps {
                script {
                    def appVersion = sh(returnStdout: true, script: './gradlew printVersion -q').trim()
                    def image = "tsmonger/s-pdf:$appVersion"
		    sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa dockerrunner@192.168.1.101 sudo docker run -it $image'
                }
            }
        }
    }
}
