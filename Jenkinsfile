pipeline {
    agent any

    tools{
        maven 'local maven'
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
                        sh """
                        scp -i "C:/Users/godkn/tomcat-demo.pem" "${WORKSPACE}/webapp/target/webapp.war" ec2-user@54.252.28.8:/home/ec2-user/
                        ssh -i "C:/Users/godkn/tomcat-demo.pem" ec2-user@54.252.28.8 "sudo mv /home/ec2-user/webapp.war /usr/share/tomcat9/webapps/ && sudo chown tomcat:tomcat /usr/share/tomcat9/webapps/webapp.war"
                        """
                    }
                }

                stage('Deploy to Production'){
                    steps{
                        sh """
                        scp -i "C:/Users/godkn/tomcat-demo.pem" "${WORKSPACE}/webapp/target/webapp.war" ec2-user@13.238.184.22:/home/ec2-user/
                        ssh -i "C:/Users/godkn/tomcat-demo.pem" ec2-user@13.238.184.22 "sudo mv /home/ec2-user/webapp.war /usr/share/tomcat9/webapps/ && sudo chown tomcat:tomcat /usr/share/tomcat9/webapps/webapp.war"
                        """
                    }
                }
            }
        }
    }
}
