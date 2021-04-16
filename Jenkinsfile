pipeline {

  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/mhmtarik/spring-crud.git', branch: 'main', credentialsId: 'mhmtarik-github-user-token'])
      }
    }
    stage('Compile') {

      steps {
        sh "mvn compile"
      }
    }
    stage('Unit Test') {

      steps {
        sh "mvn test"
      }
    }

    stage('Building package') {
      steps {
        sh "mvn clean package"
      }
    }
    stage('Building image') {
      steps {
        sh "docker build -t spring-crud:$BUILD_NUMBER ."
      }
    }
    stage('Run Application in container') {
      steps {
        sh "docker run -d -p 0.0.0.0:70:7000 spring-crud:$BUILD_NUMBER"
      }
    }

    stage('Cleanup Container Stack') {
      steps {
        sh "docker rm -f \$(docker ps -a -q)"
      }
    }

  }
}
