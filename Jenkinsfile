stage 'Checkout'
 node() {
  deleteDir()
  checkout scm
 }

stage 'Build em Dev'
 node () {
  openshiftBuild(buildConfig: 'devapp', showBuildLogs: 'true')
 }

stage 'Check Build'
 node () {
  openshiftVerifyBuild(buildConfig: 'devapp')
 }

stage 'Tests'
 node () {
  sh 'echo testando antes de promover para Prod'
  sh 'checando minha Super Variável com OC CLI!'
  sh 'echo `oc env dc/devapp --list | grep SuperVar | cut -d \= -f2`'
 }

stage 'Aprovação'
 node () {
  slackSend channel: 'codehip', color: '#42e2f4', message: "CTO - Favor aprovar o Build do Projeto - ${env.JOB_NAME}"
  input 'Esta versão pode ser promovida para Produção ?'
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
