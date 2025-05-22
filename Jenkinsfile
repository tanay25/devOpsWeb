pipeline {
    agent any
    
    tools {
        maven 'jenkins_maven'
    }
    parameters {
         string(name: 'staging_server', defaultValue: '15.207.55.205', description: 'Remote Staging Server')
    }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        deploy adapters: [tomcat9(alternativeDeploymentContext: '', path: '', url: 'http://15.207.55.205:8080/')], contextPath: null, war: '**/*.war'
                    }
                }
            }
        }
    }
}
