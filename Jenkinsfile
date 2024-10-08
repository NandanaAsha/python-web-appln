pipeline {
    agent any
    
   // environment {
        // Define any environment variables here if needed
   // }
    
    stages {
        stage('Clone repo') {
            steps {
                // Checkout the source code from the repository
                git branch: 'main', url: 'https://github.com/NandanaAsha/python-web-appln.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Install dependencies from requirements.txt
                sh 'sudo yum install python3 python3-pip -y'
                sh 'pip3 install -r requirements.txt'
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run tests if available, adjust as needed
                sh 'pytest'
            }
        }
        
        stage('Package Application') {
            steps {
                // Package the application into a zip file
                sh 'zip -r app.zip app.py'
            }
        }

        stage('Build image') {
            steps {
                // Package the application into a zip file
                sh 'docker build -t pythonwebimg:V$BUILD_NUMBER .'
            }
        }
        
        stage('Deploy application to container') {
            steps {
                // Package the application into a zip file
                sh 'docker run -d --name pythonwebcont_$BUILD_NUMBER -P pythonwebimg:V$BUILD_NUMBER'
            }
        }
    }
    
    post {
        always {
            // Archive the zip file as an artifact
            archiveArtifacts artifacts: 'app.zip', allowEmptyArchive: true
        }
        
        success {
            // Additional actions on success
            echo 'Build and deployment to container succeeded!'
        }
        
        failure {
            // Additional actions on failure
            echo 'Build or deployment to container failed!'
        }
    }
}
