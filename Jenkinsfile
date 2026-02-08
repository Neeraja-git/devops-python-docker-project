pipeline {   
    agent any
    environment {
        IMAGE_NAME = "devops-python-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
}
parameters {
    string(name: 'IMAGE_NAME', defaultValue: 'devops-python-app')
    string(name: 'APP_PORT'. defaultValue: '5001')
}
    stages {
       stage('checkout code'){
          steps {
             checkout scm
}
}
       stage('Build Docker Image'){
          steps {
              sh '''
              docker build -t $IMAGE_NAME:$IMAGE_TAG .
              '''
} 
}
       stage('Run Container') {
          steps {
             sh '''
             docker rm -f python_app || true
             docker run -d \
               -p 5001:5000 \
               --name python_app \
               devops-python-app:${BUILD_NUMBER}
             '''
}
}
       stage("Health check") {
          steps {
              sh '''
              echo "waiting for application to start..."
              sleep 10
              curl -f http://host.docker.internal:5001
              '''
}
}
       stage('Push Image to DockerHub') {
          steps {
              withCredentials([usernamePassword(
                  CredentialsID: 'neerajaj532',
                  usernameVariable: 'DOCKER_USER',
                  passwordVariable: 'DOCKER_PASS'
       )]){
           sh '''
             echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
             docker tag devops-python-app:${BUILD_NUMBER} $DOCKER_USER/devops-python-app:${BUILD_NUMBER}
             docker push $DOCKER_USER/devops-python-app:${BUILD_NUMBER}
           '''
}
}
}       
       stage('Deploy to kubernetes') {
          steps {
             sh '''
               kubectl apply -f deployement.yaml
               kubectl apply -f service.yaml
              '''
}
}
}
     post {
        always {
             sh '''
             echo "Cleaning unused Docker resources..."
             dock
er image prune -f
             '''
}
}
}
