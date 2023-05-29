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
sudo apt update
sudo apt install -y build-essential zlib1g-dev libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt1-dev libsqlite3-dev sqlite3
wget https://cache.ruby-lang.org/pub/ruby/3.2/ruby-3.2.0.tar.gz
tar -xf ruby-3.2.0.tar.gz
cd ruby-3.2.0
./configure --prefix=/usr/local
make
sudo make install
ruby --version
'''
      }
    }

  }
}