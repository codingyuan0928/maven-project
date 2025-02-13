pipeline {
    agent any
    tools {
        maven 'local maven'
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Arching....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging'){
            steps{
                build job:'deploy-to-staging'
            }
        }
        stage('Deploy to production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'是否影響到生產環境?'
                }
                build job:'deploy-to-production'
            }
            post{
                success{
                    echo '程式碼影響到生產環境'
                }
                failure{
                    echo '影響失敗'
                }
            }
        }
    }
}
