pipeline {
    agent any

    environment {
        APP_NAME = "vite-react-app"
        BUILD_DIR = "dist"
        ZIP_FILE = "app.zip"
        VM_USER = "Administrator"
        VM_IP = "10.0.2.15"
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
                        scp -o StrictHostKeyChecking=no $ZIP_FILE $VM_USER@$VM_IP:C:/deploy/
                        ssh $VM_USER@$VM_IP "
                            powershell -Command \"
                                Expand-Archive -Path 'C:\\deploy\\$ZIP_FILE' -DestinationPath '$VM_PATH' -Force
                                iisreset
                            \"
                        "
                    """
                }
            }
        }
    }
}
