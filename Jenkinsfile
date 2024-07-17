pipeline {
    agent any

    stages {
        stage('hello') {
            steps {
                echo "Hello World"
            }
        }
        stage('Build C Program') {
            steps {
                script {
                    // Assuming the main C file is named main.c
                    try {
                        // Compile the C program
                        sh 'gcc -o main main.c'
                        
                        // Run the compiled program to check for runtime errors
                        sh './main'
                        
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
                        User: ${env.GIT_COMMITTER_NAME} has committed to 
                        Branch: ${env.GIT_BRANCH} 
                        Build Status: ${currentBuild.result}
                    """,
                    subject: 'Build Notification',
                    to: 'khaledwaleed42069@gmail.com'
                }
            }
        }
    }
}
