pipeline {
    agent any

    environment {
        GIT_COMMITTER_NAME = sh(script: 'git log -1 --pretty=format:%cn', returnStdout: true).trim()
        GIT_BRANCH = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
    }

    stages {
        stage('hello') {
            steps {
                echo "Hello World"
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
                    to: 'yousef.elsawy2003@gmail.com, yousefsawy5@gmail.com, gn3li2002@gmail.com'
                }
            }
        }
    }
}
