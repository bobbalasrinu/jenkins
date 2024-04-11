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
                
                git branch:"${params.branch_build}", url: 'https://github.com/bobbalasrinu/jenkins.git'
            }
        }

         stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACT",
                    url: "https://mvnartifactory.jfrog.io/",
                    credentialsId: "jenkins"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACT",
                    releaseRepo: 'jenkins-libs-release-local',
                    snapshotRepo: 'jenkins-libs-snapshot-local'
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
                    tool: MVN, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
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