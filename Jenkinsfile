pipeline {
    agent any
   
   environment {
    KUBECONFIG = '/var/jenkins_home/.kube/config'
          }
    stages {
        stage('Validate Kubernetes Files') {
            steps {
                sh 'kubectl apply --dry-run=client -f k8s/'
            }
        }

        stage('Deploy Edge-UPF to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/namespace.yaml'
                sh 'kubectl apply -f k8s/edge-upf-demo.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }

        stage('Verify Edge-UPF Deployment') {
            steps {
                sh 'kubectl get pods -n edge-upf'
                sh 'kubectl get svc -n edge-upf'
            }
        }

        stage('Self-Healing Check') {
            steps {
                sh 'kubectl rollout status deployment/edge-upf -n edge-upf'
		sh '''
		kubectl delete pod -n edge-upf -l app=edge-upf || true
                sleep 10
                kubectl get pods -n edge-upf
		'''

            }
        }
    }
}
