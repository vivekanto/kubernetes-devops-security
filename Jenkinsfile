pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true" //in order to download later
              archive 'target/*.jar' 
            }
        }   
      stage('Unit Tests - JUnit and Jacoco') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }
      stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t vivekanto/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push vivekanto/numeric-app:""$GIT_COMMIT""'
        }
      }
    }  
  }
}