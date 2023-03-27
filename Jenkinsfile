pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    } 
    stages{
        stage("sonar quality check"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token-1') {
                        sh "chmod +x gradlew"
                        sh "./gradlew sonarqube"
        stage("quality gate")
            steps{
                timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
            }
                    }
                }
            }
        }
    }
}        
 
}

