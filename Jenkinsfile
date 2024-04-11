pipeline {
    agent { label 'JDK8-MVN'}
    parameters{
        choice(name:'branch_build', choices:['main','master'], description: 'branch to build')
    }
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('vcs'){
            steps{
                 mail subject: 'Build Started', 
                body: 'Build Started', 
                to: 'bsrinivas@gmail.com' 
                git branch:"${params.branch_build}", url: 'https://github.com/bobbalasrinu/jenkins.git'
            }
        }
        stage('build') {
            steps{
                sh 'export PATH="/usr/lib/jvm/java-8-openjdk-amd64/bin:$PATH" && mvn clean package'
            }
        post {
        always {
            echo 'Job completed'
            mail subject: 'Build Completed', 
                  body: 'Build Completed', 
                  to: 'bsrinivas@gmail.com'
        }
        failure {
            mail subject: 'Build Failed', 
                  body: 'Build Failed', 
                  to: 'bsrinivas@gmail.com' 
        }
        success {
            junit '**/surefire-reports/*.xml'
        }
    } 
  }
    }
}