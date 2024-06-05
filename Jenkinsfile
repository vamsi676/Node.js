pipeline {
    agent any
    environment {
        PROJECT_ID = 'careful-granite-425318-v7'
        CLUSTER_NAME = 'cluster-1'
        LOCATION = 'us-central1-c'
        CREDENTIALS_ID = 'a73e5197-8888-419e-8e67-f3ae62d75218'
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
                    docker.withRegistry('', DOCKER_PASS) {
                        myimage = docker.build("surya8899/hello:${env.BUILD_ID}")
                    }

                    docker.withRegistry('', DOCKER_PASS) {
                        echo "Push Docker Image"
                        myimage.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                // Replace the image tag in deployment.yaml using PowerShell
                powershell """
                    (Get-Content -path deployment.yaml) -replace 'hello:latest', 'hello:${env.BUILD_ID}' | Set-Content -path deployment.yaml
                """
                // Use the Kubernetes Engine Builder step for deployment
                step([
                    $class: 'KubernetesEngineBuilder', 
                    projectId: env.PROJECT_ID, 
                    clusterName: env.CLUSTER_NAME, 
                    location: env.LOCATION, 
                    manifestPattern: 'deployment.yaml', 
                    credentialsId: env.CREDENTIALS_ID, 
                    verifyDeployments: true
                ])
            }
        }
    }
}
