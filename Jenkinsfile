
  // parameters {
  //   booleanParam(name: 'ROLLBACK', defaultValue: false, description: '¿Ejecutar rollback?')
  // }

pipeline {
  agent any

  stages {
    stage('Rollback') {
      steps {
        script {
          def userInput = input(
            id: 'userInputRollback',
            message: '¿Deseas realizar un rollback?',
            parameters: [
              booleanParam(name: 'ROLLBACK', defaultValue: false, description: '¿Ejecutar rollback?')
            ]
          )

          if (userInput == true) {
            echo 'Rollback activado'
            // Aquí podrías ejecutar comandos de rollback si lo deseas
          } else {
            echo 'Rollback no activado'
          }
        }
      }
    }
    
    stage('Verificar docker') {
      steps {
        sh 'docker info'
      }
    }

    stage('Docker build') {
      steps {
        sh 'docker build -t app_image:${env.BUILD_NUMBER} .'
      }
    }

    stage('Test') {
      steps {
        sh 'docker run --rm app_image:${env.BUILD_NUMBER} ./vendor/bin/phpunit tests'
      }
    }

    stage('Impresion de mensaje') {
      steps {
        sh 'echo HelloWorld'
      }
    }
  }
}


    // stage('Rollback') {
    //   when {
    //     expression {
    //       // Puedes activar rollback con una condición o una variable, aquí es un ejemplo
    //       return params.ROLLBACK == true
    //     }
    //   }
    //   steps {
    //     script {
    //       // Supón que el rollback debe usar la imagen anterior (BUILD_NUMBER - 1)
    //       def previousBuild = env.BUILD_NUMBER.toInteger() - 1
    //       sh """
    //         echo "Realizando rollback a la versión: ${previousBuild}"
    //         docker stop jenkins_container || true
    //         docker rm jenkins_container || true
    //         docker run -d --name jenkins_container jenkins1:${previousBuild}
    //       """
    //     }
    //   }
    // }
