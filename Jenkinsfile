pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                sshagent(credentials: ['github-ssh']) {
                    sh 'git --version'
                    sh 'rm -rf .git || true'   // 워크스페이스 꼬임 방지(선택)
                    git branch: 'main',
                        url: 'git@github.com:alstjd2627/source-maven-java-spring-hello-webapp.git'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn -v'
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: 'tomcat',
                        url: 'http://192.168.56.152:8080'
                    )
                ],
                contextPath: 'hello-world',
                war: 'target/hello-world.war'
            }
        }
    }
}

