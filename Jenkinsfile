pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost', description: 'Production Server')

        string(name: 'tomcat_base_path', defaultValue: '/home/mm/dev/CICD/jenkins/tomcat-', description: 'Staging Server')
        string(name: 'jenkins_base_path_to_war', defaultValue: '/var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war', description: 'Production Server')  
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
                        sh "cp ${params.jenkins_base_path_to_war} ${params.tomcat_base_path}staging/webapps"
                    }
                }
                

                stage ("Deploy to Production"){
                    steps {
                        sh "cp ${params.jenkins_base_path_to_war} ${params.tomcat_base_path}prod/webapps"
                    }
                }
            }
        }
    }
}