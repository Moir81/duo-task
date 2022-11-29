pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
   
            docker build -t moir81/duo-task . 
            docker push moir81/duo-task
            '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
            kubectl apply -f duobackend.yaml
            kubectl apply -f duofrontend.yaml
                '''
            }
        }
        stage('Clean') {
            steps {
                sh '''
            docker system prune --force
                '''
            }
        }        
    }
}
