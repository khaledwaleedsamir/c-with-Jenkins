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
                    GIT_COMMITTER_NAME = '' // Initialize variable for git committer name
                    GIT_BRANCH = '' // Initialize variable for git branch
                }
            }
        }
        stage('Checkout') {
            steps {
                // Check out the code from the git repo
                checkout scm
                script {
                    // Retrieve the latest committer name and current branch name
                    bat script: 'git log -1 --pretty=format:"%cn" > committer.txt'
                    bat script: 'git rev-parse --abbrev-ref HEAD > branch.txt'

                    GIT_COMMITTER_NAME = readFile('committer.txt').trim()
                    GIT_BRANCH = readFile('branch.txt').trim()
                }
                echo "Hello from Jenkins!"
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
