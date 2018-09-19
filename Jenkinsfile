pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        echo 'build started'
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
        junit 'target/surefire-reports/*.xml'
      }
    }
    stage('Deliver') {
      steps {
        sh '''echo \'The following Maven command installs your Maven-built Java application\'
echo \'into the local Maven repository, which will ultimately be stored in\'
echo \'Jenkins\'\'s local Maven repository (and the "maven-repository" Docker data\'
echo \'volume).\''''
      }
    }
    stage('Notification') {
      parallel {
        stage('Notification') {
          steps {
            emailext(subject: 'Blue Ocean Test', body: 'Blue Ocean Test')
          }
        }
        stage('Clean') {
          steps {
            cleanWs(cleanWhenSuccess: true)
          }
        }
      }
    }
  }
}