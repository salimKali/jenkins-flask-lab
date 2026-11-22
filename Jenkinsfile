pipeline {
agent any
environment {
IMAGE_NAME = 'jenkins-flask-lab'
CONTAINER_NAME = 'jenkins-flask-app'
APP_PORT = '8081'
}
stages {
stage('Checkout') {
steps {
echo 'Source code is available in the workspace.'
}
}
stage('Build') {
steps {
sh 'docker build -t IMAGE {BUILD_NUMBER} .'
}
}
stage('Test') {
steps {
sh 'docker run --rm IMAGE {BUILD_NUMBER} pytest -v' N

AME:

}
}
stage('Deploy') {
steps {
sh '''
docker rm -f ${CONTAINER_NAME} || true
docker run -d --name ${CONTAINER_NAME} -p APP
P
ORT: 8000 − eAPP
V
ERSION =

{BUILD_NUMBER} IMAGE {BUILD_NUMBER} N

AME:

'''
}
}
stage('Verify') {
steps {
sh 'curl --fail --retry 10 --retry-delay 2 http://localhost:${APP_PORT}/health'
sh 'curl --fail http://localhost:${APP_PORT}/'
}
}
}
post {
success {
echo 'Pipeline completed successfully.'
}
failure {
echo 'Pipeline failed. Check the stage that reported an error.'
}
}
}