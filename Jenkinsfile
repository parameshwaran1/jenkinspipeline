pipeline {
    agent any
    tools {
       maven  'localMaven'
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
                       build job :"deploytostaging"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                       build job :"deploytoproduction"
                    }
                }
            }
        }
    }
}
