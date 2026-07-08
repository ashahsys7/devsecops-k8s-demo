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

      stage('Mutation Tests - PIT') {
            steps {
              sh 'mvn org.pitest:pitest-maven:mutationCoverage'
            }
            post {
              always {
                pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
              }
            }
          }


      stage('Docker Build and Push') {
              steps {
                  script {
                      withDockerRegistry([credentialsId: "docker-hub", url: ""]) {

                          sh 'printenv'

                          sh '''
                              docker build -t amitshah9/numeric-app:${GIT_COMMIT} .
                              docker push amitshah9/numeric-app:${GIT_COMMIT}
                          '''
                      }
                  }
              }
            }

    }
}