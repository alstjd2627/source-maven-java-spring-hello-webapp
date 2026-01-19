 Jenkinsfile 
pipeline {
    agent any

    triggers {
        // 매 분마다 소스 코드 변경 사항을 확인합니다.
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                // git 명령의 문법을 표준 형식으로 수정했습니다.
                git branch: 'main',
                    url: 'https://github.com/alstjd2627/source-maven-java-spring-hello-webapp.git'
            }
        }

        stage('Build') {
            steps {
                // Maven 빌드를 수행하여 war 파일을 생성합니다.
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                // Tomcat 서버로 생성된 war 파일을 배포합니다.
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
