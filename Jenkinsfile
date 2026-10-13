pipeline {
    agent any

    stages {
        stage('Check Kubernetes') {
            steps {
                sh 'kubectl get deployments'
            }
        }

        stage('Deploy Green') {
            steps {
                sh 'kubectl apply -f green-deployment.yaml'
                sh 'kubectl rollout status deployment/myapp-green --timeout=120s'
            }
        }

        stage('Approve Traffic Switch') {
            steps {
                input message: 'Switch traffic to Green?', ok: 'Switch'
            }
        }

        stage('Switch Traffic to Green') {
            steps {
                sh "kubectl patch service myapp-service -p '{\\"spec\\":{\\"selector\\":{\\"app\\":\\"myapp\\",\\"color\\":\\"green\\"}}}'"
            }
        }
    }

    post {
        success {
            echo 'Green deployment and traffic switch completed.'
        }
        failure {
            echo 'Pipeline failed. Check the Jenkins console output.'
        }
    }
}
