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
        
     // Stage for Code Analysis (SonarQube)
        // stage('CODEANALYSIS') {
        //     steps {
        //         script {
        //             echo 'Running SonarQube analysis...'
        //             withSonarQubeEnv('sonarQube') {
        //                 sh 'gradle sonarqube' 
        //             }
        //         }
        //     }
        // }

        // stage('CODEQUALITY') {
        //             steps {
        //                 script {
        //                     echo 'Checking SonarQube Quality Gate...'
        //                     def qualityGate = waitForQualityGate()
        //                     if (qualityGate.status != 'OK') {
        //                         error "SonarQube Quality Gate failed: ${qualityGate.status}"
        //                     }
        //                 }
        //             }
        //         }

         stage('BUILD') {
            steps {
                script {
                    echo 'Building the project...'
                    sh 'gradle build'
                }
            }
        }
        stage('DEPLOY') {
            steps {
                script {
                    echo 'Deploying the application...'
                    sh 'gradle publish' 
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

