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
             post {
            always {
                cucumber '**/reports/*.json'
            }
        }
        }
        
     // Stage for Code Analysis (SonarQube)
        stage('CODEANALYSIS') {
            steps {
                script {
                    echo 'Running SonarQube analysis...'
                    withSonarQubeEnv('sonar') {
                        sh 'gradle sonar' 
                    }
                }
            }
        }
        
        // Stage for Code Quality (SonarQube Quality Gate Check)
        stage('CODEQUALITY') {
                    steps {
                        script {
                            echo 'Checking SonarQube Quality Gate...'
                            def qualityGate = waitForQualityGate()
                            if (qualityGate.status != 'OK') {
                                error "SonarQube Quality Gate failed: ${qualityGate.status}"
                            }
                        }
                    }
                }

        // Stage for building the project
         stage('BUILD') {
            steps {
                script {
                    echo 'Building the project...'
                    sh 'gradle build'
                }
            }
            
        }

        stage('Archive Artifacts') {
    steps {

        archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true

        archiveArtifacts artifacts: 'build/docs/javadoc/**/*', fingerprint: true

        archiveArtifacts artifacts: 'build/reports/tests/**/*', fingerprint: true

        archiveArtifacts artifacts: 'build/reports/cucumber/**/*', fingerprint: true
    }
}

        // Stage for deploying the application
        stage('DEPLOY') {
            steps {
                script {
                    echo 'Deploying the application...'
                    sh 'gradle publish' 
                }
            }
        }
        
        // Stage for notifications with email, Slack)
        stage('NOTIFYMAIL') {
            steps {
                script {
                    currentBuild.result = currentBuild.result ?: 'SUCCESS'
                    if (currentBuild.result == 'SUCCESS') {
                        echo 'Sending success notifications...'
                        mail to: 'raouf.selidja@gmail.com',
                             subject: "Build Success: ",
                             body: ":rocket: *Deploiement termine avec succes!* :tada:"
                    } else {
                        echo 'Sending failure notifications...'
                        mail to: 'raouf.selidja@gmail.com',
                             subject: "Build Failed: ",
                             body: "The build for  failed. Check the logs for details."
                    }
                }
            }
        }
        // Stage for notifications with Slack
        stage('NOTIFYSLACK') {
            steps {
                // script {
                //     echo 'Sending message notification with slack...'
                //     // Call Gradle sendMail task
                //     sh 'gradle notifySlack'
                // }
                slackSend channel: '#web-app',
                    color: 'good',
                    message: ':rocket: *Deploiement termine avec succes!* :tada:'
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

