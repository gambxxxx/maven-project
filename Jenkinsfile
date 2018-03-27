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
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp  **/target/*.war /home/petar/Documents/apache-tomcat-8.5.29-staging/webapps"
                    }
                }
                stage('Sanity check'){
                    steps{
                      input "Does the staging environment look OK?"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "cp  **/target/*.war /home/petar/Documents/apache-tomcat-8.5.29-prod/webapps"
                    }
                }
    }
}
node {
    stage "Create build output"

    // Make the output directory.
    sh "mkdir -p output"

    // Write an useful file, which is needed to be archived.
    writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."

    // Write an useless file, which is not needed to be archived.
    writeFile file: "output/uselessfile.md", text: "This file is useless, no need to archive it."

    stage "Archive build output"

    // Archive the build output artifacts.
    archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
}
