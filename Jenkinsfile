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
    }
        stages {
        stage('checkout From SCM') {
            steps {
                git branch: 'main', credentialsId: 'token_key_github', url: 'https://github.com/nikhilve99/e2e-pipeline.git'
            }
        }
    }
}
