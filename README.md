# jenkins-sonarQube



#pipeline

```
pipeline {
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
}
```



## Remote Connection after click approve will go next stage  

```pipeline {
    agent any

    stages {
        stage('Connect Remote Server') {
            steps {
                sshagent(['95876b68-024b-4330-8090-ca2a62592b26']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no -p 20202 anshul@10.192.1.86 "
                        echo 'Connected to remote server!'
                        ls -l
                        pwd
                        mkdir jenkinsamit0200f
                        "
                    '''
                }
            }
        }

        stage('Approve Deployment') {
            steps {
                input message: 'Click "Proceed" to start deployment', ok: 'Deploy'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deployment stage running...'
                // Add your deployment commands here
            }
        }
    }
}```

 

