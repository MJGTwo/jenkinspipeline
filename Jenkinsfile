pipeline {
    agent any
    tools {
      maven 'localMaven'
      jdk 'localJDK'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:9090', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
        stage('Deploy to Staging'){
          steps {
            build job: 'Deploy-to-staging'
          }
        }

        stage('Deploy to Production'){
          steps{
            timeout(time:5, unit:'DAYS'){
              input message: 'Approve PRODUCTION deployment?'
            }

            build job: 'Deploy-to-Prod'
          }
          post {
            success {
              echo 'Code deployed to Production.'
            }
            failure {
              echo 'Deployment failed.'
            }
          }
        }
    }
}
