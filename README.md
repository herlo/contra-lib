# A general library for upstream ci

#### Example Usage:
```
package_name = env.CI_MESSAGE['name']
msgHeader = fedMsgHeader(branch: 'fed_repo', topic: '/fedMsgTopic', username: 'currentUser')
msgComplete = fedMsgComplete(header: header)
msgError = fedMsgError(header: header)

containers = ['rpmbuild', 'singlehost-test']

deployOpenShiftTemplate(containers: containers) {
    ciPipeline(buildPrefix: 'package-builds', completeMsg: msgComplete, errorMsg: msgError, package_name: package_name) {
        stage('koji-build') {
            handlePipelineStep(queuedMsg: queuedMsg, runningMsg: runningMsg) {
                executeInContainer(containerName: 'koji-build-container, containerScript: 'verify-build.sh',
                        stageVars: stageVars)
                }
            }
        }
    }
}
```