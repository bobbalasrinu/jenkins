pipeline {
    agent {label 'JDK8-MVN'}
    stages {
        stage ('Clone') {
            steps {
                git branch: 'main', url: "git@github.com:bobbalasrinu/jenkins.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACT",
                    url: 'https://mvnartifactory.jfrog.io/',
                    credentialsId: "ARTIFACT"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACT",
                    releaseRepo: ARTIFACTORY_LOCAL_RELEASE_REPO,
                    snapshotRepo: ARTIFACTORY_LOCAL_SNAPSHOT_REPO
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
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
                    goals: 'clean install',
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