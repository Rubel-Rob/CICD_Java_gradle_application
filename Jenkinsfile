pipeline{
    agent any 
    stages{
        stage("sonar qaulity check"){
            agent {
                docker{
                    image "openjdk:11"
                }
            }
        }
    }
}        