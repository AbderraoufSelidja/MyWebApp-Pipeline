pipeline {
    agent any 

    environment {
        MAVEN_REPO_USERNAME = credentials('github-selidja-abdo')
        MAVEN_REPO_PASSWORD = credentials('github-selidja-abdo')
    }

    stages {
        // Étape pour exécuter les tests
        stage('test') {
            steps {
                script {
                    echo 'Exécution des tests...'
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
        
        // Étape pour l'analyse de code (SonarQube)
        stage('codeAnalysis') {
            steps {
                script {
                    echo 'Exécution de l’analyse SonarQube...'
                    withSonarQubeEnv('sonar') {
                        sh 'gradle sonar' 
                    }
                }
            }
        }
        
        // Étape pour vérifier la qualité du code (SonarQube Quality Gate)
        stage('codeQuality') {
            steps {
                script {
                    echo 'Vérification du Quality Gate de SonarQube...'
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "Le Quality Gate de SonarQube a échoué : ${qualityGate.status}"
                    }
                }
            }
        }

        // Étape pour construire le projet
        stage('build') {
            steps {
                script {
                    echo 'Construction du projet...'
                    sh 'gradle build'
                }
            }
        }

        // Étape pour archiver les artefacts
        stage('archiveArtifacts') {
            steps {
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
                archiveArtifacts artifacts: 'build/reports/tests/**/*', fingerprint: true
                archiveArtifacts artifacts: 'build/reports/cucumber/**/*', fingerprint: true
            }
        }

        // Étape pour déployer l'application
        stage('deploy') {
            steps {
                script {
                    echo 'Déploiement de l’application...'
                    sh 'gradle publish' 
                }
            }
        }
        
        // Étape pour envoyer des notifications par e-mail
        stage('notifyMail') {
            steps {
                script {
                    currentBuild.result = currentBuild.result ?: 'SUCCESS'
                    if (currentBuild.result == 'SUCCESS') {
                        echo 'Envoi de notifications de succès...'
                        mail to: 'raouf.selidja@gmail.com',
                             subject: "Succès de la construction : ",
                             body: ":rocket: *Déploiement terminé avec succès!* :tada:"
                    } else {
                        echo 'Envoi de notifications d’échec...'
                        mail to: 'raouf.selidja@gmail.com',
                             subject: "Échec de la construction : ",
                             body: "La construction a échoué. Consultez les journaux pour plus de détails."
                    }
                }
            }
        }
        
        // Étape pour envoyer des notifications avec Slack
        stage('notifySlack') {
            steps {
                slackSend channel: '#web-app',
                    color: 'good',
                    message: ':rocket: *Déploiement terminé avec succès!* :tada:'
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé.'
        }
        success {
            echo 'Pipeline complété avec succès.'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
