pipeline {
    agent any

    parameters {
        string(name:'tomcat_dev', defaultValue: '35.182.77.140', description: 'Staging Server')
        string(name:'tomcat_prod', defaultValue:'35.183.105.251', description: 'Production Server')
    }

    triggers{
        pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage('Deploy to Staging'){
                    steps{
                        sh "scp -i Users/Ravitheja/Downloads/SSH/Canada.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                
                stage("Deploy to Production"){
                    steps {
                        sh "scp -i Users/Ravitheja/Downloads/SSH/Canada.pem **/target*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
