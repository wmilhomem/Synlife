pipeline {
  agent any
  stages {
    stage('Atualizando código fonte') {
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