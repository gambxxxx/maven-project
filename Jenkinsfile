pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '127.0.0.1:9080', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '127.0.0.1:8090', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /usr/share/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/home/petar/Documents/apache-tomcat-8.5.29-staging
/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /usr/share/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/home/petar/Documents/apache-tomcat-8.5.29-prod
/webapps"
                    }
                }
            }
        }
    }
}
