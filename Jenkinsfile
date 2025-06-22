pipeline {
  agent any

  environment {
    CAMUNDA_VERSION = "12.0.2"
  }

  stages {
    stage('Add Helm Repo') {
      steps {
        sh '''
          helm repo add camunda https://helm.camunda.io || true
          helm repo update
        '''
      }
    }

    stage('Install Camunda 8') {
      steps {
        sh '''
          helm install camunda camunda/camunda-platform \
            --version $CAMUNDA_VERSION \
            --namespace camunda \
            --create-namespace \
            -f values.yaml
        '''
      }
    }

    stage('Verify') {
      steps {
        sh 'kubectl get pods -n camunda'
      }
    }

    stage('Port Forward Operate (Optional)') {
      steps {
        sh '''
          kubectl port-forward svc/camunda-operate 8080:80 -n camunda &
          sleep 30
          curl http://localhost:8080 || true
        '''
      }
    }
  }
}
