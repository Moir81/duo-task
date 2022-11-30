pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                 script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker build -t gcr.io/lbg-python-cohort-8/moir-duotask:latest -t gcr.io/lbg-python-cohort-8/moir-duotask:$BUILD_NUMBER . 
                        docker push gcr.io/lbg-python-cohort-8/moir-duotask:$BUILD_NUMBER
                        docker push gcr.io/lbg-python-cohort-8/moir-duotask:latest
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
                        kubectl rollout restart deployment duobackend --namespace=dev
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
