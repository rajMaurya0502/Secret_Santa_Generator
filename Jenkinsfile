pipeline {
    agent any
    
     environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rajMaurya0502/Secret_Santa_Generator.git'
            }
        }
         stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
         stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
         stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                     sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Santa \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Santa '''
                 }
            }
        }
         stage('Docker Build') {
            steps {
                sh 'docker build -t secretsanta .'
            }
        }
        stage('Docker Push'){
            steps{
                script{
                withDockerRegistry(credentialsId: 'docker-cred') {
                    
                    sh 'docker tag secretsanta rajmaurya/secretsanta:latest'
                    sh  'docker push rajmaurya/secretsanta:latest'
                
                }
                }
            }
        }
        stage('Deploy'){
            steps{
                sh 'docker run -d -p 8081:8080 secretsanta:latest'
            }
        }
    }
}
