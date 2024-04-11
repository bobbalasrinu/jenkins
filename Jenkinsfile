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
                    serverId: "ARTIFACT",
                    releaseRepo: 'jenkins-libs-release-local',
                    snapshotRepo: 'jenkins-libs-snapshot-local'
                )

            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'MVN', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean package',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACT"
                )
            }
        }
    }
}