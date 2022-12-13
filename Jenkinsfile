pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/SATISH08081996/demo-counter-app.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
        stage('upload war file to nexus'){
                steps{
                    script{
                        def readPomVersion = readMavenPom file: 'pom.xml'
                        
                         def nexusRepo = readPomVersion.version.endswith ("SNAPSHOT") ? "nexusapp-snapshot" : "Nexusapp-release"
                        
                        nexusArtifactUploader artifacts: 
                        [
                            [
                                artifactId: 'springboot', 
                                classifier: '', file: 'target/Uber.jar', 
                                type: 'jar'
                            ]
                        ], 
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: '43.205.208.234:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: nexusRepo, 
                        version: "${readPomVersion.version}"
                    }
                }
             }
    }
}    
