pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }
        stage('test') {
            steps {
              sh "mvn test"
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }
        stage('Docker Build and push') {
            steps {
              sh "printenv"
              sh 'docker build -t mistersiddd/numeric-app:""$GIT_COMMIT"" .'
              sh 'docker push mistersiddd/numeric-app:""$GIT_COMMIT""'
            }
        }    
    }
}