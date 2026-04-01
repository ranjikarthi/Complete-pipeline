pipeline {
    agent any

    environment {
        IMAGE = "yourdockerhub/html-app:latest"
        SERVER = "ubuntu@your_server_ip"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Provision Server') {
            steps {
                bat '''
                wsl ansible-playbook -i ansible/inventory ansible/playbook.yml
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t %IMAGE% app
                '''
            }
        }

        stage('Push Image') {
            steps {
                bat '''
                docker login -u yourdockerhub -p yourpassword
                docker push %IMAGE%
                '''
            }
        }

        stage('Deploy Website') {
            steps {
                bat '''
                ssh %SERVER% "docker pull %IMAGE% && docker stop webapp || true && docker rm webapp || true && docker run -d -p 80:80 --name webapp %IMAGE%"
                '''
            }
        }
    }
}