pipeline {
    agent any 	
	environment {
		
		PROJECT_ID = 'sincere-muse-275812'
                CLUSTER_NAME = 'kubernetes-soma'
                LOCATION = 'europe-north1-c'
                CREDENTIALS_ID = 'gcr-soma'
		
	}
	
    stages {	
	   stage('Scm Checkout') {            
		steps {
                  checkout scm
		}	
           }
           
	   stage('Build') { 
                steps {
                  echo "Cleaning and packaging..."
                  sh 'mvn clean package'		
                }
           }
	   stage('Test') { 
		steps {
	          echo "Testing..."
		  sh 'mvn test'
		}
	   }
	   stage('Build Docker Image') { 
		
		steps {
		 	echo "Inside steps..."
                   script {
			   echo "Inside script..."
                    	   //myimage = docker.build("somaam/ssimage:${env.BUILD_ID}")
			    myimage = docker.build("eu.gcr.io/sincere-muse-275812/somaam/ssimage:${env.BUILD_ID}")
			   echo "After image.."
                   }
                }
	   }
	   stage("Push Docker Image") {
		   
                steps {
			echo "push docker steps.."
                   script {
			   echo "Inside script..."
                           //docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
			   docker.withRegistry('https://eu.gcr.io') {
                    
                            myimage.push("${env.BUILD_ID}")	
			    echo "After push..."
                     }
			   
                   }
                }
            }
	   
           stage('Deploy to K8s') { 
                steps{
                   echo "Deployment started ..."
		   sh 'ls -ltr'
		   sh 'pwd'
		   sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
                   step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
		   echo "Deployment Finished ..."
            }
          }
    }
}
