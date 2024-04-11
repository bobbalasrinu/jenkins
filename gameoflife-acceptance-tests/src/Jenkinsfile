pipeline {
    agent { label 'JDK8-MVN' }
    options { 
        timeout(time: 1, unit: 'HOURS')
    }
    stages {
        stage('Source Code') {
            steps {
                git url: 'https://github.com/bobbalasrinu/jenkins.git', 
                branch: 'main'
            }
        }
         stage('Artifactory-Configuration') {
            steps {
                rtMavenDeployer (
                    id: 'spc-deployer',
                    serverId: 'ARTIFACT',
                    releaseRepo: 'jenkins-libs-release-local',
                    snapshotRepo: 'jenkins-libs-release-local'

                )
            }
        }
         stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'MVN', 
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                   
                )
            }
        }
    }
}