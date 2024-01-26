pipeline {

    agent any

    stages {

        stage("Git Checkout"){
            steps {
                git branch: 'main', url: 'https://github.com/vishalchauhan91196/java-app.git'
            }
        }

        stage("Unit Testing"){
            steps {
                sh 'mvn test'
            }
        }

        stage("Integration Testing"){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage("Build"){
            steps {
                sh 'mvn clean install'
            }
        }   

        stage("Static Code Analysis"){
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqube_token') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage("Quality Gate Analysis"){
            steps {
                script {
                   waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube_token' 
                }
            }
        }

        stage("Docker image build"){
            steps {
                sh'''
                    docker build -t javaappcicd:v1.$BUILD_ID .
                    docker image tag javaappcicd:v1.$BUILD_ID vishalchauhan9/javaappcicd:v1.$BUILD_ID 
                    docker image tag javaappcicd:v1.$BUILD_ID vishalchauhan9/javaappcicd:latest
                '''
            }
        }

        stage("Push docker image to Docker Hub"){
            steps {
                withCredentials([string(credentialsId: 'dockerhub_password', variable: 'dockerhub_creds')]) {
                    sh'''
                        docker login -u vishalchauhan9 -p ${dockerhub_creds}
                        docker image push vishalchauhan9/javaappcicd:v1.$BUILD_ID
                        docker image push vishalchauhan9/javaappcicd:latest              
                    '''
                }
            }
        }
    }
}