pipeline {   
    agent any
    environment {
        IMAGE_NAME = "devops-python-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
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
              sleep 5
                   
              APP_IP=$(docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' python_app)

              echo "App IP is $APP_IP"
              curl -f http://$APP_IP:5000
              '''
}
}
}
     post {
        always {
             sh '''
             echo "Cleaning unused Docker resources..."
             docker image prune -f
             '''
}
}
}
