stage 'Checkout'
 node() {
  deleteDir()
  checkout scm
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
  sh 'echo testando antes de promover para prod'
 }

stage 'Aprovação'
 node () {
  input 'Esta versão pode ser promovida para Produção ?' ok: 'YEAH!''
}

stage 'Tag to Prod'
 node () {
   openshiftTag(srcStream: "devapp", srcTag: "latest", destStream: "devapp", destTag: "prod")
}

stage 'Prod Check'
 node () {
  openshiftVerifyDeployment(deploymentConfig: 'prodapp')
}

stage 'slack notification'
  node () {
   sh 'git log -1 --pretty=%B > commit-log.txt'
   GIT_COMMIT=readFile('commit-log.txt').trim()
   slackSend channel: 'codehip', color: '#1e602f', message: "BUILD_Terminado: PROJETO - ${env.JOB_NAME} - (${GIT_COMMIT})"
 }
