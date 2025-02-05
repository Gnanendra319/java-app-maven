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
            def dockerImage = docker.image("${DOCKER_IMAGE}")
            docker.withRegistry('https://index.docker.io/v1/', "docker") {
                dockerImage.push()
            }
        }
      }
    }
    stage('Update Deployment File') {
        environment {
            GIT_REPO_NAME = "maven-jenkins-ArgoCD"
            GIT_USER_NAME = "mdazfar2"
        }
        steps {
            withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                sh '''
                    git config user.email "mdazfaralam440@gmail.com"
                    git config user.name "mdazfar2"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" manifest_file/deployment.yml
                    git add manifest_file/deployment.yml
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                '''
            }
        }
    }
  }
}
