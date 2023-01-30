pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw clean verify -DskipTests=true sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.host.url=http://172.31.18.122:9000 \\
  -Dsonar.login=sqp_0da7ca5996c7f8e7271c6c1d3e20bbcde112a7e8'''
      }
    }

    stage('Unit test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          agent {
            node {
              label 'Test Env'
            }

          }
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('Performance Testing') {
          agent {
            node {
              label 'Test Env'
            }

          }
          steps {
            sh './mvnw verify'
          }
        }

      }
    }

  }
}