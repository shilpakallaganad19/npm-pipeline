node {
    def server = Artifactory.server 'Artifactory'
    def rtNpm = Artifactory.newNpmBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/shilpakallaganad19/npm-pipeline.git'
    }

    stage ('Artifactory configuration') {
        rtNpm.deployer repo: 'npm-local', server: server
        rtNpm.resolver repo: 'npm-remote', server: server
        rtNpm.tool = 'Nodejs-1' // Tool name from Jenkins configuration
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Install npm') {
        rtNpm.install buildInfo: buildInfo, path: '.', args: --silent
    }

    stage ('Publish npm') {
        rtNpm.publish buildInfo: buildInfo, path: '.'
    }

}
