pipeline {
    agent any
    tools{
        jdk 'Java17'
        maven 'Maven3'
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
    }
}
