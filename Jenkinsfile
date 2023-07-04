pipeline {
    agent any 
    tools {
        maven 'maven'
    
    }
    stages {
	
	stage('Checkout Code from Git') {
               steps {
        git branch: 'main', url: 'https://github.com/Ramopshub/java-spring-boot-maven.git'
    }
   }
        stage('Compile and Clean') { 
            steps {
                // Run Maven on a Unix agent.
              
                sh "mvn clean compile"
            }
        }
        stage('deploy') { 
            
            steps {
                sh "mvn package"
            }
        }
        stage('Build Docker image'){
          
            steps {
                echo "Hello Ram"
                sh 'ls'
                sh 'docker build -t  ramvn/docker_jenkins_springboot:${BUILD_NUMBER} .'
            }
        }
        stage('Docker Login'){
            
            steps {
                 withCredentials([string(credentialsId: 'DockerID9', variable: 'Dockerpwd')]) {
                    sh "docker login -u ramvn -p ${Dockerpwd}"
                }
            }                
        }
        stage('Docker Push'){
            steps {
                sh 'docker push ramvn/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd -p  8081:8080 ramvn/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
	 stage('Deploy') {
      steps {
        // Deploy to Kubernetes cluster
        withKubeConfig([credentialsId: 'eks-cred', serverUrl: 'https://CDC73CD7A8CF370E71F7D1AEFDC0EBE9.gr7.ap-south-1.eks.amazonaws.com']) {
          sh 'kubectl apply -f ekxctl.yaml'
        }
      }
    }
}

