node {
    def server = Artifactory.server SERVER_ID
    def rtNpm = Artifactory.newNpmBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/shilpakallaganad19/npm-pipeline.git'
    }

    stage ('Artifactory configuration') {
        rtNpm.deployer repo: 'npm-local', server: server
        rtNpm.resolver repo: 'npm-remote', server: server
        rtNpm.tool = NPM_TOOL // Tool name from Jenkins configuration
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Install npm') {
        rtNpm.install buildInfo: buildInfo, path: 'npm-example'
    }

    stage ('Publish npm') {
        rtNpm.publish buildInfo: buildInfo, path: 'npm-example'
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
