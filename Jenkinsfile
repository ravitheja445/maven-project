pipeline {
    agent any
    stages{
        stage('build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving ....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage (' Deploy-to-staging') {
            steps {
                build job: 'Deploy-to-staging'
            }
        }
        stage ('Deploy-to-production'){
            steps{
                timeout(time=5,unit:'DAYS')
                input message: ' Approve PRODUCTION Deployment ?' , submitter : admin
            }

            build job: 'Deploy-to-production'
        }
        post{
            success {
                echo 'Code deployed to Production.'
            }
            failure {
                echo 'Deployment failed.'
            }
        }        
    }
}
