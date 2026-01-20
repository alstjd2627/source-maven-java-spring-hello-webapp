pipeline {
   agent none

   stages {
      stage('CheckOut') {
          agent any
	  steps {
	     git branch: 'main',
	     url: 'https://github.com/alstjd2627/source-maven-java-spring-hello-webapp.git'
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
            withDOckerResistry(credentialsId: 'docker-hub-token', url: 'https://index.docker.io/v1/') {
            sh 'docker image push minseong22/tomcat:$BUILDNUMBER'
            sh 'docker image push minseong22/tomcat:latest'
            }
         }
      }
      stage('Running Container') {
         agent {
            docker { image 'docker:dind' }
         }
         steps {
            sh 'docker -H tcp://192.168.56.154:2375 run -d --name webserver -p 80:8080 minseong22/tomcat:latest'
            sh 'docker image tag tomcat:hello minseong22/tomcat:latest'
         }
      }
   }
}
	
