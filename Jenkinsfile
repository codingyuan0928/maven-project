pipeline {
    agent any

    tools{
        maven 'local maven'
    }

    environment {
        WORKSPACE_PATH = "C:\ProgramData\Jenkins\.jenkins\workspace\FullyAutomated"
    }

    parameters{
        string(name: 'tomcat_dev', defaultValue: '54.252.28.8', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '13.238.184.22', description: 'Production Server')
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
                    echo '開始存檔...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments'){
            parallel{
                stage('Deploy to Staging'){
                    steps{
                        sh "scp -i C:/Users/godkn/tomcat-demo.pem ${env.WORKSPACE_PATH}/webapp/target/webapp.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat9/webapps"
                    }
                }

                stage('Deploy to Production'){
                    steps{
                        sh "scp -i C:/Users/godkn/tomcat-demo.pem ${env.WORKSPACE_PATH}/webapp/target/webapp.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
                    }
                }
            }
        }
    }
}
