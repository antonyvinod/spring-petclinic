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
        sh '''mvn clean verify sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.host.url=http://localhost:9000 \\
  -Dsonar.login=sqp_0da7ca5996c7f8e7271c6c1d3e20bbcde112a7e8'''
      }
    }

  }
}