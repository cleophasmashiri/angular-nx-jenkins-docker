pipeline {
    agent any
    stages {
        stage('Install Dependencies') { 
            steps {
                sh 'npm ci'
                sh 'npm run cy:verify'
            }
        }
        stage('Build') { 
            steps {
                sh 'npm run build'
            }
        }
        stage('Test') { 
            steps {
                sh 'npm run ci:cy-run'
            }
        }
    }
}
