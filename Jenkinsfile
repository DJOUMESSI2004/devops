pipeline {
    agent any

    environment {
        APP_NAME = "vite-react-app"
        BUILD_DIR = "dist"
        ZIP_FILE = "app.zip"
        VM_USER = "Administrator"
        VM_IP = "10.210.64.63"
        VM_PATH = "C:/inetpub/wwwroot"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/DJOUMESSI2004/devops.git'
            }
        }

        stage('Build with Docker') {
            steps {
                sh '''
                    docker build -t vite-react-builder .
                    docker create --name temp-builder vite-react-builder
                    docker cp temp-builder:/app/dist ./dist
                    docker rm temp-builder
                '''
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r app.zip dist'
            }
        }

        stage('Deploy to IIS') {
            steps {
                sshagent(['vm-ssh-key']) {
                    sh """
                        scp $ZIP_FILE $VM_USER@$VM_IP:C:/deploy/
                    """
                }
            }
        }
    }
}
