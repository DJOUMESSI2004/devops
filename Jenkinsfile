pipeline {
    agent any

    environment {
        APP_NAME = 'vite-react-app'
        BUILD_DIR = 'dist'
        ZIP_FILE = 'app.zip'
        VM_USER = 'Administrator'
        VM_IP = '10.210.64.63'
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
            # Build the Docker image
            docker build -t my-build-app .

            # Remove any existing container with the name "temp-builder"
            docker rm -f temp-builder || true  # || true ensures the command doesn't fail if no container exists

            # Create the container
            docker create --name temp-builder my-build-app

            # Copy the contents of /app/dist from the container to the Jenkins workspace
            docker cp temp-builder:/app/dist/. ./dist

            # Remove the container after copying the files
            docker rm temp-builder
        '''
            }
        }

        stage('Package') {
            steps {
                sh 'cd dist && zip -r ../../app.zip .'
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

        stage("Clean up"){
            steps {
                sh 'rm -f app.zip'
            }
        }
    }
}
