pipeline {
    agent any // This specifies hat the pipeline can run on any available agen

    environment {
        MAVEN_REPO_USERNAME = credentials('github-selidja-abdo')
        MAVEN_REPO_PASSWORD = credentials('github-selidja-abdo')
    }



    stages {
        // Stage for running tests
        stage('TEST') {
            steps {
                script {
                    echo 'Running tests...'
                    // Run your tests here, e.g. using Gradle or Maven
                    sh 'chmod +x gradle'
                    sh 'gradle test'
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
}
