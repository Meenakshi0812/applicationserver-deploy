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

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'mkdir -p ${remoteFolderPath}'"
                    sh "zip -r ${clonePath}/${zipFileName} ${clonePath}/*"
                    sh "scp -o StrictHostKeyChecking=no ${clonePath}/${zipFileName} ubuntu@3.86.97.100:${remoteFolderPath}/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'unzip -o ${remoteFolderPath}/${zipFileName} -d ${remoteFolderPath}'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'ln -sfn ${remoteFolderPath} /var/www/html/code'"
                }
            }
        }
    }
}
