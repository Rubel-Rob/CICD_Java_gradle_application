pipeline {
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
    } 
    stages {
        stage ("sonar quality check") {
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-password') {
                        sh "chmod +x gradlew"
                        sh "./gradlew sonarqube"  
                    }
                }
            }
        }
        stage ("docker build and docker push") {
            steps {
                script {
                withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_pass')]) {
                    sh '''
                        docker build -t 3.86.95.162:8083/springapp:${VERSION} .
                        docker login -u admin -p $docker_pass 3.86.95.162:8083
                        docker push 3.86.95.162:8083/springapp:${VERSION}
                        docker rmi 3.86.95.162:8083/springapp:${VERSION}
                    '''        
                }
            }
        }
    }
}

}
