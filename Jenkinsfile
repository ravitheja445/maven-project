pipeline{
    agent any
    stages{
        stage('build'){
            steps {
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to staging'){
            steps{
                build job: 'Staging'
            }
        }
    }
}