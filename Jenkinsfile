pipeline {
    agent { label 'JDK8-MVN' }
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
        stage('vcs') {
            steps {
                git url: 'git@github.com:bobbalasrinu/jenkins.git',
                    branch: 'main'
            }
        }
        stage('build and package') {
            steps {
                 rtMavenDeployer (
                    id: "SPC_DEPLOYER",
                    serverId: "ARTIFACT",
                    releaseRepo: 'jenkins-libs-release-local',
                    snapshotRepo: 'jenkins-libs-snapshot-local/'
                )
                rtMavenRun (
                    tool: 'MVN', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "SPC_DEPLOYER"
                    //,
                    //buildName: "${JOB_NAME}",
                    //buildNumber: "${BUILD_ID}"
                )
                rtPublishBuildInfo (
                    serverId: "ARTIFACT"
                )
            }
        }
        stage('reporting') {
            steps {
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
            }
        }
    }

}