pipeline {
  agent any

  environment {
    CAMUNDA_VERSION = "12.1.0"
    CUSTOM_PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin"
  }

  stages {
    stage('Add Helm Repo') {
      steps {
        sh '''
          export PATH=$CUSTOM_PATH:$PATH
          helm repo add camunda https://helm.camunda.io || true
          helm repo update
        '''
      }
    }

    stage('Install Camunda 8') {
      steps {
        sh '''
          export PATH=$CUSTOM_PATH:$PATH
          helm install camunda camunda/camunda-platform \
            --version $CAMUNDA_VERSION \
            --namespace default \
            --create-namespace \
            -f values.yaml
        '''
      }
    }

    stage('Verify') {
      steps {
        sh '''
          export PATH=$CUSTOM_PATH:$PATH
          kubectl get pods -n camunda
        '''
      }
    }

  }
}
