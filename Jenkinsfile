pipeline {
    agent any

    environment {
        GIT_COMMITTER_NAME = ''
        GIT_BRANCH = ''
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    GIT_COMMITTER_NAME = bat(script: 'git log -1 --pretty=format:"%%cn"', returnStdout: true).trim()
                    GIT_BRANCH = bat(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                }
            }
        }
        stage('hello') {
            steps {
                echo "Hello World"
            }
        }
        stage('Build C Program') {
            steps {
                script {
                    try {
                        // Compile the C program
                        bat 'gcc -o main main.c'
                        
                        // Run the compiled program to check for runtime errors
                        bat '.\\main'
                        
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error "Build failed: ${e.message}"
                    }
                }
            }
        }
        stage('Email') {
            steps {
                script {
                    emailext body: """
                        User: ${GIT_COMMITTER_NAME} has committed to 
                        Branch: ${GIT_BRANCH} 
                        Build Status: ${currentBuild.currentResult}
                    """,
                    subject: 'Build Notification',
                    to: 'khaledwaleed42069@gmail.com'
                }
            }
        }
    }
}
