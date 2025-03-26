# jenkins-sonarQube



#pipeline

```pipeline {
    agent any
    tools{
        jdk  'jdk21'
        // maven  'maven3'
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-queue'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'dev-main', credentialsId: 'fa734e19-87ec-473a-9d55-5dfbec986f40', url: 'https://amitkumarprajapati@bitbucket.org/zcsprojects/zoho-saas.git'
             }
        }
        // stage('COMPILE') {
        //     steps {
        //         sh "mvn clean compile -DskipTests=true"
        //     }
        // }
        
        //   stage('OWASP Scan') {
        //     steps {
        //         dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP'
        //         dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }
        
        stage('Sonarqube') {
            steps {
              withSonarQubeEnv('sonar-queue') {
            sh '''
                $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=zoho-saas \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=zoho-saas \
            '''
        }
            }
        }
        
        // stage('cloning process') {
        //     steps {
        //         sh "docker-compose up"
        //     }
        // }
    }
}```
