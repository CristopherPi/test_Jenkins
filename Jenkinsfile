pipeline {
  agent any 

  parameters {
    booleanParam(name: 'ROLLBACK', defaultValue: false, description: '¿Ejecutar rollback?')
    string(name: 'ROLLBACK_BUILD_NUMBER', defaultValue: '', description: 'Número de build al que deseas hacer rollback (ej: 42)')
  }

  stages {
    stage('Verificar docker') {
      steps {
        sh 'docker info'
      }
    }

    stage('Docker build') {
      steps {
        sh 'docker build -t jenkins1:${env.BUILD_NUMBER} .'
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
            docker stop jenkins_container || true
            docker rm jenkins_container || true
            docker run -d --name jenkins_container jenkins1:${rollbackBuild}
          """
        }
      }
    }
  }
}
