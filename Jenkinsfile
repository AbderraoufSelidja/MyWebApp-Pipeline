pipeline {
    agent any // This specifies that the pipeline can run on any available agent

    environment {
        MAVEN_REPO_USERNAME = credentials('74cf92a5-30b2-458a-8425-cbac9c667ba3')  // Replace with your actual Jenkins credential ID
        MAVEN_REPO_PASSWORD = credentials('74cf92a5-30b2-458a-8425-cbac9c667ba3')  // Replace with your actual Jenkins credential ID
    }

    stages {
        // Stage for running tests
        stage('TEST') {
            steps {
                script {
                    echo 'Running tests...'
                    // Run your tests here, e.g. using Gradle or Maven
                    sh 'chmod +x gradlew'
                    sh 'gradlew test'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
