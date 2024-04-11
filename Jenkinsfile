pipeline {
    agent { label 'JDK8-MVN'}
    stages {
        stage('vcs'){
            steps{
                git branch:'main', url: 'https://github.com/bobbalasrinu/jenkins.git'
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