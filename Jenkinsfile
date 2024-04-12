pipeline {
    agent {label 'JDK8-MVN'}
    options {
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'JDK8'
    }
    stages {
        stage ('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/bobbalasrinu/jenkins.git'
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "JFROG_APR",
                    releaseRepo: 'jenkins-libs-release-local'
                )

            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'MVN', 
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG_APR"
                )
            }
        }
    }
}