pipeline {
    agent any

    environment {
        APP_NAME = 'vite-react-app'
        BUILD_DIR = 'dist'
        ZIP_FILE = 'app.zip'
        VM_USER = 'Administrator'
        VM_IP = '192.168.1.38'
        VM_PATH = 'C:/inetpub/wwwroot'
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
                    docker build -t my-build-app .
                    docker create --name temp-builder my-build-app
                    docker cp temp-builder:/app/dist/. ./dist  // Copy the contents of /app/dist directly to ./dist in the Jenkins workspace
                    docker rm temp-builder
                '''
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r app.zip dist'  // Zip the contents of the dist folder without any nested dist folder
            }
        }

        stage('Deploy to IIS') {
            steps {
                sshagent(['vm-ssh-key']) {
                    sh """
                    scp $ZIP_FILE $VM_USER@$VM_IP:C:/deploy/
                    ssh $VM_USER@$VM_IP powershell -Command \\
                        "\\\$ProgressPreference = 'SilentlyContinue'; Expand-Archive -Path 'C:\\\\deploy\\\\$ZIP_FILE' -DestinationPath 'C:\\\\inetpub\\\\wwwroot' -Force; iisreset"
                    """
                }
            }
        }
    }
}
