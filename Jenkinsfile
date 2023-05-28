pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            echo 'Atualizando arquivos modificados'
          }
        }

        stage('Install') {
          steps {
            sh 'ls -la'
          }
        }

      }
    }

  }
}