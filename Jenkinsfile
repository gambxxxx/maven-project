pipeline {
    agent any
    def staging_server='127.0.0.1:9080'
    def prodcution_server='127.0.0.1:8090'
    parameters {
         string(name: 'DEPLOY_ENV', defaultValue: ${staging_server}, description: 'Staging Server')
         string(name: 'DEPLOY_ENV', defaultValue: ${production_server}, description: 'Production Server')
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
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp  **/target/*.war /home/petar/Documents/apache-tomcat-8.5.29-staging/webapps"
                    }
                }
                /*stage('Sanity check'){
                    steps{
                      input "Does the staging environment look OK?"
                    }
                }*/
                stage ("Deploy to Production"){
                    steps {
                        sh "cp  **/target/*.war /home/petar/Documents/apache-tomcat-8.5.29-prod/webapps"
                    }
                }
      }
}
