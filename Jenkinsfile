pipeline {
    agent any
	tools {
		maven 'Maven'
	}
	
	environment {
		PROJECT_ID = 'santu0908-365206'
                CLUSTER_NAME = 'autopilot-cluster-1'
                LOCATION = 'us-central1'
                CREDENTIALS_ID = 'kubernetes'		
	}
	
        
        stages{

              stage('Quality Gate Status Check'){
		      agent {
                docker {
                 image 'maven:3-openjdk-11'

                }
            }
                  steps{
                      script{
			      withSonarQubeEnv('sonarserver') { 
			      sh "mvn clean sonar:sonar"
                       	     	}
			      timeout(time: 1, unit: 'HOURS') {
			      def qg = waitForQualityGate()
				      if (qg.status != 'OK') {
					   error "Pipeline aborted due to quality gate failure: ${qg.status}"
				      }
                    		}
		    	   
                 	}
               	 }  
              }	
			       	     	         
    
	    stage('Scm Checkout') {
		    steps {
			    checkout scm
		    }
	    }
	    
	    stage('Build') {
		    steps {
			    sh 'mvn clean package'
		    }
	    }
	    
	    stage('Test') {
		    steps {
			    echo "Testing..."
			    sh 'mvn test'
		    }
	    }

	
	    
            }
	}