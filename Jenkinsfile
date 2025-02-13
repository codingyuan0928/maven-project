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
                    echo '開始存檔....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}
