/*pipeline{
     agent any
     stages{
     stage('Init'){
                  steps
                       {
                          echo "testing"
                       }
                  }
     stage('Build'){
                   steps
                        {
                            echo "build"

                        }
                  }


     stage('deploy'){
                      steps 
                           {
                             echo "code deployed"
                           }
                    }
   }

}*/

pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost:9090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:8090', description: 'Production Server')
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
                        sh "cp -i  **/target/*.war aman@${params.tomcat_dev}:/home/aman/webstaging/webapps"
                    }
                }
                 
                stage ("Deploy to Production"){
                    steps {
                        sh "cp -i **/target/*.war aman@${params.tomcat_prod}:/home/aman/webproduction/apache-tomcat-8.5.51/webapps"
                    }
                }
            }
        }
    }
}
