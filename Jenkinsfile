pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'node:14' // Specify the Docker image you want to use
        NX_CLI = 'nx'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('your-angular-nx-image')
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    docker.image('your-angular-nx-image').inside {
                        sh 'npm ci'
                    }
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    docker.image('your-angular-nx-image').inside {
                        def apps = sh(script: "${NX_CLI} show projects", returnStdout: true).trim().split('\n')
                        for (app in apps) {
                            sh "${NX_CLI} lint ${app}"
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image('your-angular-nx-image').inside {
                        def apps = sh(script: "${NX_CLI} show projects", returnStdout: true).trim().split('\n')
                        for (app in apps) {
                            sh "${NX_CLI} test ${app}"
                        }
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.image('your-angular-nx-image').inside {
                        def apps = sh(script: "${NX_CLI} show projects", returnStdout: true).trim().split('\n')
                        for (app in apps) {
                            sh "${NX_CLI} build ${app}"
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        docker.image('your-angular-nx-image').inside {
                            def apps = sh(script: "${NX_CLI} show projects", returnStdout: true).trim().split('\n')
                            for (app in apps) {
                                sh "${NX_CLI} deploy ${app}"
                            }
                        }
                    } else {
                        echo 'Not deploying as this is not the main branch'
                    }
                }
            }
        }
    }

    post {
        always {
            docker.image('your-angular-nx-image').inside {
                junit '**/test-results/*.xml'
                archiveArtifacts artifacts: '**/dist/**', allowEmptyArchive: true
                cleanWs()
            }
        }
    }
}
