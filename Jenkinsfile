pipeline {
  agent any
  stages {
    stage('Atualizando c�digo fonte') {
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