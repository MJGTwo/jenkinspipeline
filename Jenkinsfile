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

stages{
  stage('Deployments'){
    parallel{
      stage('Deploy to Staging'){
        steps {
          sh "scp -i /Users/Shared/Jenkins/Home/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
        }
      }

      stage('Depoly to Production'){
        steps {
          sh "scp -i /Users/Shared/Jenkins/Home/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
        }
      }
    }
  }
}
}
