pipeline {
    agent any 
    
    tools{
        jdk 'jdk11'
        maven 'maven'
    }
    
    environment {
        SCANNER_HOME='opt/sonar-scanner-4.8.1.3023-linux/conf'
    }
    
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git branch: 'branch1', changelog: false, poll: false, url: 'https://github.com/Bala-1997/Petclinic.git'
            }
        }
        
        stage("Compile"){
            steps{
                sh "JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/ mvn clean compile"
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/ mvn test"
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/ $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
    
                }
            }
        }
        /*
        stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./ --format HTML ', odcInstallation: 'DP'
                dependencyCheckPublisher pattern: '**//*dependency-check-report.xml'
            }
        }
        */
       
        stage("Build") {
            steps {
                sh "JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/ mvn clean package"
            }
        }
       /* 
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: '58be877c-9294-410e-98ee-6a959d73b352', toolName: 'docker') {
                        
                        sh "docker build -t image1 ."
                        sh "docker tag image1 adijaiswal/pet-clinic123:latest "
                        sh "docker push adijaiswal/pet-clinic123:latest "
                    }
                }
            }
        }
        
        stage("TRIVY"){
            steps{
                sh " trivy image adijaiswal/pet-clinic123:latest"
            }
        }
       */ 
        
        stage("Deploy To Tomcat"){
            steps{
                sh "cp  /var/lib/jenkins/workspace/jenkinsfile/target/petclinic.war /opt/apache-tomcat-9.0.67/webapps/ "
            }
        }
    }
}
