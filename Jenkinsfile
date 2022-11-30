pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                 script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker build -t moir81/duo-task:latest -t moir81/duo-task:$BUILD_NUMBER . 
                        docker push moir81/duo-task:$BUILD_NUMBER
                        docker push moir81/duo-task:latest
                        '''
                    }  else {
                        sh "echo 'Push not required!'"
                    }
                }
             
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        kubectl apply -f duobackend.yaml --namespace=dev
                        kubectl apply -f duofrontend.yaml --namespace=dev
                        '''
                    }  else {
                        sh '''
                        kubectl apply -f duobackend.yaml 
                        kubectl apply -f duofrontend.yaml 
                        '''
                    }
                }
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
