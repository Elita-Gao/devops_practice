pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // 拉取代码
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // 使用 Dockerfile 构建镜像
                    docker.build("my-app-image:${env.BUILD_ID}", ".")
                }
            }
        }

        stage('Push Image') {
            when {
                expression { env.BUILD_SOURCE == 'tag' }  // 可选：仅在有 tag 时推送
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credential') {
                        docker.image("my-app-image:${env.BUILD_ID}").push()
                    }
                }
            }
        }
    }
}
