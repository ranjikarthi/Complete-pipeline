pipeline {
    agent any

    environment {
        IMAGE = "yourdockerhub/html-app:latest"
        SERVER = "ranji@172.20.102.67"
        SSH = "C:\\Windows\\System32\\OpenSSH\\ssh.exe"
        KEY = "C:\\ProgramData\\Jenkins\\.jenkins\\.ssh\\id_rsa"
    }

    stages {

        stage('Provision Server') {
            steps {
                bat """
                %SSH% ^
                -i %KEY% ^
                -o StrictHostKeyChecking=no ^
                -o BatchMode=yes ^
                %SERVER% ^
                "if ! command -v docker > /dev/null 2>&1; then sudo -n apt-get update -y && sudo -n apt-get install -y docker.io; fi"
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE% app"
            }
        }

        stage('Push Docker Image') {
            steps {
                bat """
                docker login -u yourdockerhub -p yourpassword
                docker push %IMAGE%
                """
            }
        }

        stage('Deploy Website') {
            steps {
                bat """
                %SSH% ^
                -i %KEY% ^
                -o StrictHostKeyChecking=no ^
                -o BatchMode=yes ^
                %SERVER% ^
                "docker pull %IMAGE% && docker stop webapp || true && docker rm webapp || true && docker run -d -p 80:80 --name webapp %IMAGE%"
                """
            }
        }
    }
}
