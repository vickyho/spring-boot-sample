pipeline {
  agent any
  stages {
    stage('checkout project') {
      steps {
        checkout scm
      }
    }
    stage('test') {
      steps {
        sh 'mvn test'
        junit 'surefire-reports'
      }
    }
    stage('package') {
      parallel {
        stage('package') {
          steps {
            sh 'mvn package'
            archiveArtifacts 'target/spring-boot-sample-data-rest-0.1.0.jar'
          }
        }
        stage('report') {
          steps {
            sh 'mvn cobertura:cobertura test'
            cobertura(coberturaReportFile: 'target/site/cobertura/coverage.xml')
          }
        }
      }
    }
    stage('deploy') {
      steps {
        sh 'make deploy-default'
        archiveArtifacts 'target/spring-boot-sample-data-rest-0.1.0.jar'
      }
    }
  }
  post {
    always {
      echo 'I will always say Hello again!'
      sh 'docker-compose run clean'

    }

    success {
      echo 'success!'

    }

    failure {
      echo 'failure!'

    }

  }
}