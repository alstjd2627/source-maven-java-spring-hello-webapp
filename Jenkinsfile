pipeline {
  agent none

  stages {
    stage('Checkout') {
      agent any
      steps {
        sshagent(credentials: ['github-ssh']) {
          sh 'rm -rf .git || true'
          git branch: 'main',
              url: 'git@github.com:alstjd2627/source-maven-java-spring-hello-webapp.git'
        }
      }
    }

    stage('Build') {
      agent {
        docker { image 'maven:3-openjdk-17' }
      }
      steps {
        sh 'mvn clean package'
      }
    }

    stage('ImageBuild') {
      agent any
      steps {
        sh 'docker image build -t tomcat:hello .'
      }
    }

    stage('ImageTag') {
      agent any
      steps {
        sh 'docker image tag tomcat:hello minseong22/tomcat:$BUILD_NUMBER'
        sh 'docker image tag tomcat:hello minseong22/tomcat:latest'
      }
    }

    stage('ImagePush') {
      agent any
      steps {
        withDockerRegistry(credentialsId: 'docker-hub-token', url: 'https://index.docker.io/v1/') {
          sh 'docker image push minseong22/tomcat:$BUILD_NUMBER'
          sh 'docker image push minseong22/tomcat:latest'
        }
      }
    }

    stage('Running Container') {
      agent any
      steps {
        sh 'docker -H tcp://192.168.56.154:2375 run -d --name webserver -p 80:8080 minseong22/tomcat:latest'
      }
    }
  }
}

