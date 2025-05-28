pipeline {
  agent any 

  parameters {
    booleanParam(name: 'ROLLBACK', defaultValue: false, description: '¿Ejecutar rollback?')
    string(name: 'ROLLBACK_BUILD_NUMBER', defaultValue: '', description: 'Número de build al que deseas hacer rollback (ej: 42)')
  }

  stages {
    stage ('Verificar docker'){
      steps {
        sh 'docker info'
      }
    }
  

    stage ('Docker buiild '){
      steps{
        sh 'docker build -t jenkins1:${BUILD_NUMBER} .'
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


  stage('Rollback') {
      when {
        expression {
          return params.ROLLBACK == true && params.ROLLBACK_BUILD_NUMBER?.trim()
        }
      }
      steps {
        script {
          def rollbackBuild = params.ROLLBACK_BUILD_NUMBER.trim()
          echo "Ejecutando rollback al build número: ${rollbackBuild}"

          sh """
            docker run -d --name jenkins_container jenkins1:${rollbackBuild}
          """
        }
      }
    }
  }

}


  
