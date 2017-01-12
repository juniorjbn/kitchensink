stage 'Checkout'
 node() {
  deleteDir()
  checkout scm
 }

stage 'slack notification'
 node () {
  sh 'git log -1 --pretty=%B > commit-log.txt'
  GIT_COMMIT=readFile('commit-log.txt').trim()
  slackSend channel: 'codehip', color: '#1e602f', message: "BUILD_INICIADO: PROJETO - ${env.JOB_NAME} - (${GIT_COMMIT})"
}

stage 'STG-Deploy'
 node () {
  openshiftBuild(buildConfig: 'devapp', showBuildLogs: 'true')
 }

stage 'STG-Check'
 node () {
  openshiftVerifyBuild(buildConfig: 'devapp')
 }

stage 'Tests'
 node () {
  sh 'echo testando '
 }
