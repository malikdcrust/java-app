pipeline {
    agent any

    stages {
        stage("GIT CHECKOUT") {
            steps{
                git branch: 'main', url: 'https://github.com/malikdcrust/java-app.git'
            }
        }
        stage("UNIT TEST") {
            steps{
                sh 'mvn test'
            }
        }
        stage("INTEGRATION TEST") {
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage("MAVEN BUILD") {
            steps{
                sh 'mvn clean install'
            }
        }
        /*stage("STATIC CODE ANALYSIS") {
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonaram') {
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
            }
        }*/
    }
}
