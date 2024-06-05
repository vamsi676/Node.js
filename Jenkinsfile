pipeline {
    agent any
    environment {
        PROJECT_ID = 'careful-granite-425318-v7'
        CLUSTER_NAME = '<<Your GKE Cluster Name>>'
        LOCATION = '<<Your GKE Cluster Location>>'
        CREDENTIALS_ID = 'multi-k8s'
        DOCKER_USER = "surya8899"
        DOCKER_PASS = 'dockerhub'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/vamsi676/Node.js'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        myimage = docker.build("surya8899/hello:${env.BUILD_ID}")
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        echo "Push Docker Image"
                        myimage.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}



