pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                 script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker build -t moir81/duotask:latest -t moir81/duotask:$BUILD_NUMBER . 
                        docker push moir81/duotask:$BUILD_NUMBER
                        docker push moir81/duotask:latest
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
                        kubectl rollout restart deployment backend --namespace=development
                        kubectl apply -f duofrontend.yaml --namespace=dev
                        '''
                    }  else {
                        sh '''
                        kubectl apply -f duobackend.yaml 
                        kubectl rollout restart deployment backend
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
