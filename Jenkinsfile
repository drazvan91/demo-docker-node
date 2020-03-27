pipeline {
    environment {
        registry = "192.168.1.108:5000/otherweb"
        dockerImage = ""
    }

    agent any

    stages {
        stage("Checkout Source") {
            steps {
                git "https://github.com/drazvan91/demo-docker-node.git"
            }
        }

        stage("Initial stage") {
            steps {
                echo "hello there"
                cat /etc/docker/daemon.json
            }
        }

        stage("Build image") {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage("Push image"){
            steps {
                script{
                    docker.withRegistry(""){
                        dockerImage.push()
                    }
                }
            }
        }

        stage("Deploy App"){
            steps {
                script {
                    kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
                }
            }
        }
    }
}