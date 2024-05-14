pipeline {
    agent any 
    
    tools{
        jdk 'jdk11'
        maven 'Maven3'
    }
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'hhttps://github.com/nithindevops/petclinic.git'
            }
        }
        
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonarqube-server') {
                    sh ''' $SCANNER_HOME/bin/sonarqube-scanner -Dsonar.projectName=Petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
    
                }
            }
        }        
                
         stage("Build"){
            steps{
                sh " mvn clean install"
            }
        }
        
       
       
        stage("Deploy To Tomcat"){
            stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', url: 'http://ec2-13-51-69-22.eu-north-1.compute.amazonaws.com:8081/manager/text', path: '/')], war: 'target/*.war'
            }
        }
    }
}
