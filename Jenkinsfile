pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    } 
    stages{
        stage ("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token-1') {
                        sh "chmod +x gradlew"
                        sh "./gradlew sonarqube"
                    
                    }
                }
            }

        }

    }
        stage ("docker build and docker push")
           steps {
               withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_pass')]) {
                           sh '''
                              docker build -t 100.24.6.192:8083/springapp:${VERSION} .
                              docker login -u admin $docker_pass 100.24.6.192:8083
                              docker push 100.24.6.192:8083/springapp:${VERSION}
                              docker rmi 100.24.6.192:8083/springapp:${VERSION}


                              '''        
               }
            
                
             }
}                   