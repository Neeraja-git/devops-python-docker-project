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
             /* for i in {1..10}; do
              //    if curl -f http:localhoste:5001; then
               //       echo "Application is bhbealthy"
                //      exit 0
                  fi
                  echo "Retrying..."
                  sleep 5
                   
             done*/
              sleep 5
              echo "Checking application health ionside container..."
              docker exec python_app curl -f http://localhost:5000
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
