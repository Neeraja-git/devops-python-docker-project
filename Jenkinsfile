pipeline {   
    agent any

    stages {
       stage('checkout code'){
          steps {
             checkout scm
}
}
       stage('Build Docker Image'){
          steps {
              sh 'docker build -t devops-python-app .'
} 
}
       stage('Run Container') {
          steps {
             sh '''
             docker rm -f python_app || true
             docker run -d -p 5010:5000 --name python_app devops-python-app
             '''
}
}
}
}
