pipeline {
    agent any

    /*parameters {
         string(name: 'dev', defaultValue: 'dev', description: 'Dev Server')
         string(name: 'test', defaultValue: 'test', description: 'Test Server')
    }*/
    triggers {
         pollSCM('*/5 * * * *')
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
        /*
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp  /target/*.war /home/petar/Documents/apache-tomcat-8.5.29-staging/webapps"
                    }
                }
                stage('Sanity check'){
                    steps{
                      input "Does the staging environment look OK?"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "cp  /target/*.war /home/petar/Documents/apache-tomcat-8.5.29-prod/webapps"
                    }
                }
                */
                stage ("Deploy to Environment"){
                    steps {
                        sh "cp  **/target/*.war /home/petar/Documents/tomcat-$params.Environment/webapps"
                    }
                }
      }
}
