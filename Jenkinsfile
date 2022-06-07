pipeline {
    agent {
        label "buildpipe"
    }
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/86vishnu/jenkins-docker-maven-java-webapp.git'
            }
        }
        
        
        
        
        stage('Build Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        
        
        
        stage('Build Docker Own image') {
            steps {
                sh  "sudo docker build -t vishu27/testimage:${BUILD_TAG} . "
            }
        }
        
        
        
            stage('Push image to Docker HUB') {
                steps {
                 withCredentials([string(credentialsId: 'DOCKER_HUB_PWD', variable: 'DOCKER_HUB_PASS_CODE')]) {
    // some block
                 
                sh "sudo docker login -u vishu27 -p $DOCKER_HUB_PASS_CODE"
                
                sh "sudo docker push vishu27/testimage:${BUILD_TAG}"
                 }
                 }
            }
            
            
            
            stage('Deploy weAPP in DEV ENV') {
                steps {
                    sh "sudo docker rm -f myapp1"
                    sh "sudo docker run -d -p 8080:8080  --name myapp1  vishu27/testimage:${BUILD_TAG}"
                }
            }
            
            
            stage('Deploy weAPP in QAT ENV'){
                steps {
                    sshagent(['QAT_PASSWORD']) {
    // some block
                   sh "ssh  -o StrictHostKeyChecking=no ec2-user@43.204.237.130  sudo docker rm -f myapp1"
                   
                   sh "ssh  ec2-user@43.204.237.130 sudo docker run -d  -p 8080:8080  --name myapp1  vishu27/testimage:${BUILD_TAG}"
                }
                
                }
            }
            
            stage('Approved') {
                steps {
                    input(message: "u want to release this to production  env ?..")
                }
            }
    
    }
}
