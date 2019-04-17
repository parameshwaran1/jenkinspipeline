pipeline {
    agent any
    tools {
       maven  'localmvn'
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
                       timeout(time:5 , unit:'DAYS'){
		          input message :"Mohak Approve Production Deployment"
                       }
                       build job :"deploytoproduction"
                    }
                    post{
                    
                       success {
                          echo "Mohak job deployed to production SUCCESS"
                       }
                       failure {
		          echo "Mohak job deployed to production FAILED"
                       }
                    }
                }
            }
        }
    }
}

