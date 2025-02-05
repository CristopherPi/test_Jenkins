pipeline {
  agent any 

  stages {
    stage ('Verificar docker'){
      steps {
        sh 'docker info'
      }
    }
  

    stage ('Docker buiild '){
      steps{
        sh 'docker build -t jenkins1 .'
      }
    }

    stage('Test'){
      steps{
        sh 'docker run jenkins1 ./vendor/bin/phpunit tests'
      }
    }
  

    stage('Impresion de mensaje'){
      steps{
        sh 'echo HelloWorld'
    }
    }
  }

}


  
