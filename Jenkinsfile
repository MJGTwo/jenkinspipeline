pipeline {
    agent any
    tools {
      maven 'localMaven'
      jdk 'localJDK'
    }

    parameters {
      string(name:'tomcat_dev', defaultValue: '54.184.43.87', description: 'Staging Server')
      string(name:'tomcat_prod', defaultValue: '52.43.218.241', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

     stage('Deployments'){
       parallel{
         stage('Deploy to Staging'){
           steps {
             sh "scp -i /Users/michaelgardner/dev/test/jenkinspipe/ec2/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
           }
         }

         stage('Depoly to Production'){
           steps {
             sh "scp -i /Users/michaelgardner/dev/test/jenkinspipe/ec2/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
           }
         }
       }
     }

// stages{
//         stage('Build'){
//             steps {
//                 sh 'mvn clean package'
//             }
//             post {
//                 success {
//                     echo 'Now Archiving...'
//                     archiveArtifacts artifacts: '**/target/*.war'
//                 }
//             }
//         }
//         stage('Deploy to Staging'){
//           steps {
//             build job: 'deploy-to-staging'
//           }
//         }
//
//         stage('Deploy to Production'){
//           steps{
//             timeout(time:5, unit:'DAYS'){
//               input message: 'Approve PRODUCTION deployment?'
//             }
//
//             build job: 'deploy-to-prod'
//           }
//           post {
//             success {
//               echo 'Code deployed to Production.'
//             }
//             failure {
//               echo 'Deployment failed.'
//             }
//           }
//         }
//     }
}
