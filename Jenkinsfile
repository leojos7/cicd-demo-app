#!groovy
node {

    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.newServer url: 'http://52.167.164.0:8081/artifactory', username: 'admin', password: 'paul@steve'
    // Create an Artifactory Maven instance.
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo

    stage('Clone sources') {
        checkout scm
    }

    stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
        rtMaven.tool = "Maven-3.5.4"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    }

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean package -o'
    }

    stage('Publish build info') {
        server.publishBuildInfo buildInfo
    }

}