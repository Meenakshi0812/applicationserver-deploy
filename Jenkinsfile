pipeline {
    agent any

    stages {
        stage('Clone Git Repository') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def clonePath = "/home/ubuntu/${timestamp}"
                    
                    dir(clonePath) {
                        git branch: 'main', url: 'https://github.com/Meenakshi0812/applicationserver-deploy.git'
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def folderName = "code_${timestamp}"
                    def zipFileName = "${folderName}.zip"
                    def clonePath = "/home/ubuntu/${timestamp}"
                    def remoteFolderPath = "/var/www/html/${folderName}"

                    sh "scp -o StrictHostKeyChecking=no -r ${clonePath}/* ubuntu@3.86.97.100:${remoteFolderPath}/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'cd ${remoteFolderPath} && zip -r /var/www/html/${zipFileName} *'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'unzip -o /var/www/html/${zipFileName} -d /var/www/html'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'ln -sfn ${remoteFolderPath} /var/www/html/code'"
                }
            }
        }
    }
}
