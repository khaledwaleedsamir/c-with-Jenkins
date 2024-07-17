pipeline {
    agent any

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
