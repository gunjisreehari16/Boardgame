pipeline {
    agent any
    tools  {
    jdk "jdk17"
    maven "maven3"
}
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage("git checkout"){
            steps {
                git branch: 'main', credentialsId: 'git_cred', url: 'https://github.com/gunjisreehari16/Boardgame.git'
            }
        }
        stage("maven compile"){
            steps {
                sh 'mvn compile'
            }
        }
        stage('maven test'){
            steps {
                sh 'mvn test'
            }
        }
    
        stage('sonarqube-scanner'){
                       steps {
                
               withSonarQubeEnv('sonar'){
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=BoardGame -Dsonar.projectkey=BoardGame -Dsonar.java.binaries=. '''
                    
                }
           
            }
            }
        stage('quality gate'){
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
            }
        }
        stage('build package'){
            steps {
                sh 'mvn package'
            }
        }

        
        
        stage("docker build and tag"){
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhubcred', toolName: 'docker') {
                sh 'docker build -t gsreehari/boardgame:latest .'
  
                }
                }
            }
        }
  
        stage('Docker push'){
            steps {
                script {
                         withDockerRegistry(credentialsId: 'dockerhubcred', toolName: 'docker') {
                         sh ' docker push gsreehari/boardgame:latest ' 
                       }
                }
             
            }
        }
        
        }
        }
        
    
        
    
    
