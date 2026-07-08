pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }   

      stage('Unit Tests Junit and JaCoCO') {
            steps {
              sh "mvn test"
            }
              post {
              always {
              // Publish JUnit test results
              junit 'target/surefire-reports/*.xml'

              // Publish JaCoCo coverage report
              jacoco(
                  execPattern: 'target/jacoco.exec'
              )
            }
          }
        }   

    }
}