pipeline {
    agent { label 'JDK8-MVN'}
    parameters{
        choice(name:'branch_build', choices:['main','master'], description: 'branch to build')
    }
    stages {
        stage('vcs'){
            steps{
                git branch:"${params.branch_build}", url: 'https://github.com/bobbalasrinu/jenkins.git'
            }
        }
        stage('path') {
            steps{
                sh 'export PATH="/usr/lib/jvm/java-8-openjdk-amd64/bin:$PATH"'
            }
        }
        stage('build') {
            steps{
                sh 'mvn package'
            }
        }
        stage('archive artifacts') {
            steps{
                junit '**/surefire-reports/*.xml'
            }
        }
    }
}