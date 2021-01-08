pipeline {
    agent any
    tools{
        maven 'localMAVEN'
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
                build job: 'Deploy-to-staging'
            }
        }
        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve this Production Deployment?'
                }
                build job: 'Deploy-to-Prod'
            }
            post {
                success {
                    echo 'Code has been deployed to Production'
                }
                failure {
                    echo 'Deployment has failed.'
                }
            }
        }

    }
}