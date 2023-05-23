pipeline {
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
    }
    stages {
        stage("sonar quality check") {
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
        stage("docker build and docker push") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_pass')]) {
                        sh """
                            docker build -t 54.165.35.36:8083/springapp:${VERSION} .
                            docker login -u admin -p ${docker_pass} 54.165.35.36:8083
                            docker push 54.165.35.36:8083/springapp:${VERSION}
                            docker rmi 54.165.35.36:8083/springapp:${VERSION}
                        """
                    }
                }
            }
        }
        stage("pushing the helm charts to nexus") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_pass')]) {
                        dir('kubernetes/') {
                            sh """
                                helmversion=\$(helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ')
                                tar -czvf myapp-\${helmversion}.tgz myapp/
                                curl -u admin:\${docker_pass} http://54.165.35.36:8081/repository/helm-hosted/ --upload-file myapp-\${helmversion}.tgz -v
                            """
                        }
                    }
                }
            }
        }
    }
}

