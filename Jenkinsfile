pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        sh 'echo passed'
        git branch: 'main', url: 'https://github.com/Gnanendra319/java-app-maven'
      }
    }
    stage('Build and Test') {
      steps {
        sh 'ls -ltr'
        // build the project and create a JAR file
        sh 'mvn install'
        sh 'echo maven build complated'
        sh 'ls -ltr'
        sh 'pwd'
      }
    }
    stage('Build and Push Docker Image') {
      steps {
        script {
          sh 'docker ps'
          sh 'docker images'
            sh 'docker build -t test:1.0.1 .'
              sh 'docker login -u <hindusree444@gmail.com> -p <Jayavijaya@1> <https://hub.docker.com/repository/docker/hindusreedunaboyina/maven-java/general>'
                 sh 'docker push hindusree444@gmail.com/maven-java:v1'
            // def dockerImage = docker.image("${DOCKER_IMAGE}")
            
           // docker.withRegistry('https://index.docker.io/v1/', "docker") {
                dockerImage.push()
            }
        }
      }
    }
}
