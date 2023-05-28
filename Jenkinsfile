pipeline {
  agent any
  stages {
    stage('Atualizando codigo fonte') {
      steps {
        echo 'Atualizando arquivos modificados'
      }
    }

    stage('Preparando ambiente') {
      steps {
        sh '''pwd
ls -la
ruby -v'''
      }
    }

  }
}