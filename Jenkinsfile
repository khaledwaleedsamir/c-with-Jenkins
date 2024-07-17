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
                    emailext body: "User: ${env.GIT_COMMITTER_NAME} has committed to Branch: ${GIT_BRANCH} Build Status: Sucess",
                    subject: 'Build Notification',
                    to: 'yousef.elsawy2003@gmail.com, yousefsawy5@gmail.com, gn3li2002@gmail.com'
                }
            }
        }
    }
}
