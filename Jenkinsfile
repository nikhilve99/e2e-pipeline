pipeline {
    agent any
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    environment{
        APP_NAME = 'e2e-pipeline'
        RELEASE = '1.0.0'
        DOCKER_USER = 'test'
        IMAGE_NAME = '${APP_NAME}' + '/' + '${DOCKER_USER}'
        IMAGE_TAG = '${RELEASE}:${BUILD_NUMBER}' 
    }
    stages {
        stage('CleanUp WorkSpace') {
            steps {
                cleanWs()
            }
        }    
         stage('checkout From SCM') {
            steps {
                git branch: 'main', credentialsId: 'token_key_github', url: 'https://github.com/nikhilve99/e2e-pipeline.git'            
            }
        } 
        stage('Build Application') {
            steps {
                sh 'mvn clean package'            
            }
        }
        stage('Test Application') {
            steps {
                sh 'mvn test'            
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                withSonarQubeEnv(credentialsId: 'sonar-api') {
                    sh 'mvn sonar:sonar'
                    }
                }            
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }

        }
        stage("ECR login") {
            steps {
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 444939732285.dkr.ecr.ap-south-1.amazonaws.com'
            }

        }  
    }
}
