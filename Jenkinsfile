pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/shilpakallaganad19/npm-pipeline.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "https://vigneshs.jfrog.io/vignesh",
                    credentialsId: "Artifactory_admin"
                )

                rtNpmResolver (
                    id: "NPM_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    repo: "npm-remote"
                )

                rtNpmDeployer (
                    id: "NPM_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    repo: "npm-local"
                )
            }
        }

        stage ('Exec npm install') {
            steps {
                rtNpmInstall (
                    tool: "Nodejs-1", // Tool name from Jenkins configuration
                    path: ".",
                    resolverId: "NPM_RESOLVER"
                )
            }
        }

        stage ('Exec npm publish') {
            steps {
                rtNpmPublish (
                    tool: "Nodejs-1", // Tool name from Jenkins configuration
                    path: ".",
                    deployerId: "NPM_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}
