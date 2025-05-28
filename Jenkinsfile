pipeline {
  agent any 

  parameters {
    booleanParam(name: 'ROLLBACK', defaultValue: false, description: '¿Ejecutar rollback?')
  }
  environment {
    DOCKER_IMAGE = "jenkins1:${BUILD_NUMBER}"
  }
  
  stages {
    stage('Verificar docker') {
      steps {
        sh 'docker info'
      }
    }

    stage('Docker build') {
      steps {
        sh 'docker build -t jenkins1:${BUILD_NUMBER} .'
      }
    }

    stage('Test') {
      steps {
        sh 'docker run --rm jenkins1:${BUILD_NUMBER} ./vendor/bin/phpunit tests'
      }
    }

    stage('Impresion de mensaje') {
      steps {
        sh 'echo HelloWorld'
      }
    }

    stage('Rollback') {
      when {
        expression {
          // Puedes activar rollback con una condición o una variable, aquí es un ejemplo
          return params.ROLLBACK == true
        }
      }
      steps {
        script {
          // Supón que el rollback debe usar la imagen anterior (BUILD_NUMBER - 1)
          def previousBuild = env.BUILD_NUMBER.toInteger() - 1
          sh """
            echo "Realizando rollback a la versión: ${previousBuild}"
            docker stop jenkins_container || true
            docker rm jenkins_container || true
            docker run -d --name jenkins_container jenkins1:${previousBuild}
          """
        }
      }
    }
  }


}
