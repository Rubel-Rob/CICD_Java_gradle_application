pipeline{
    agent any 
    stages{
        stage('sonar quality check')
           steps{
               script{
                   withSonarQubeEnv(credentialsId: 'sonar-token-1') {
                          sh 'chmod +x gradlew'
                          sh './gradlew soanrqube'
                   }
                }
             }
        }
    }