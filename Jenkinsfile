pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/DiniduEkanayake/Simple-Node-App'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t diniduekanayake/mynodeapp:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'samin-docker', variable: 'samindocker')]) {
                    script {
                        bat "docker login -u adomicarts -p %samindocker%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push diniduekanayake/mynodeapp:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
