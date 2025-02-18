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
                bat 'mvn clean package'
            }

            post{
                success{
                    echo '開始存檔...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

       stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        script {
                            // Define variables for staging server
                            def warFilePath = "${env.WORKSPACE}\\webapp\\target\\webapp.war"
                            def sshKeyPath = 'C:\\Users\\godkn\\tomcat-demo.pem'
                            def remoteUser = 'ec2-user'
                            def remoteHost = params.tomcat_dev
 
                            // Debugging output
                            echo "WAR File Path: ${warFilePath}"
                            echo "SSH Key Path: ${sshKeyPath}"
 
                            // SCP command to transfer the .war file to the staging server
                            bat """
                            scp -i "${sshKeyPath}" "${warFilePath}" ${remoteUser}@${remoteHost}:/var/lib/tomcat9/webapps/"""
                        }
                    }
                }
                                stage('Deploy to Production') {
                    steps {
                        script {
                            // Define variables for production server
                            def warFilePath = "${env.WORKSPACE}\\webapp\\target\\webapp.war"
                            def sshKeyPath = 'C:\\Users\\godkn\\tomcat\\tomcat-demo.pem'
                            def remoteUser = 'ec2-user'
                            def remoteHost = params.tomcat_prod
 
                            // SCP command to transfer the .war file to the production server
                            bat """
                            scp -i "${sshKeyPath}" "${warFilePath}" ${remoteUser}@${remoteHost}:/var/lib/tomcat9/webapps/"""
                        }
                    }
                }
            }
        }
    }
}
