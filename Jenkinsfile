pipeline {
    agent any
    
    tools {
        maven 'localMAVEN'
    }

    parameters { 
         string(name: 'tomcat_dev', defaultValue: '54.211.31.199', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.85.17.74', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '/usr/share/jenkins/jenkins.war'
                }
            }
        }
 
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/john/tomcat.ppk /usr/share/jenkins/jenkins.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/john/tomcat.ppk /usr/share/jenkins/jenkins.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}