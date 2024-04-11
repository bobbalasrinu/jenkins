pipeline {
    agent { lable 'JKD8-MVN'}
    stages {
        stage('vcs'){
            step{
                git branch:'main', url: 'https://github.com/bobbalasrinu/jen_pipe.git'
            }
        }
        stage('build') {
            steps{
                sh 'export PATH="/usr/lib/jvm/java-8-openjdk-amd64/bin:$PATH" && mvn package'
            }
        }
        stage('archive artifacts') {
            steps{
                junit '**/surefire-reports/*.xml'
            }
        }
    }
}