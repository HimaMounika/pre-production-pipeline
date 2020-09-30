// Warning: This file is manaintained by DevOps Engineers. Please don't make changes in this file.
pipeline {
  agent { label 'pls-bld-l01' }
  options { timestamps () }
  parameters {
  string defaultValue: 'develop', description: '"Enter Branch Name"', name: 'BRANCH_NAME', trim: true
  choice choices: ['None', 'pls-aps-t03', 'pls-aps-t04', 'pls-aps-t05', 'pls-aps-t06', 'pls-aps-p01'], description: '''None -> Do not deploy \n pls-aps-t03 -> Test server \n pls-aps-t04 -> Test server 2 \n pls-aps-t05 -> AWS Test server \n pls-aps-t06 -> AWS Test server 2 \n pls-aps-p01 -> Acc server ''', name: 'environment'
  booleanParam                defaultValue: false, description: 'Check to remove pagecache', name: 'InvalidatePagecache'
  }
  environment {
	def server = "${params.environment}"
  }
  
    stages {
 
         stage('check User') {
               steps {
                
                wrap([$class: 'BuildUser']) {
                  script {
                     USER_ID = "${BUILD_USER_ID}"
                  }
                }
                
              }	
        }
 
 /** Temp disabled sonarqube
	stage('Sonarqube') {
  
		environment {
			scannerHome = tool 'sonar-scanner'
			jdk = tool name: 'jdk-pls-bld-l01'
			JAVA_HOME = "${jdk}"
		}
    
		steps {

			withSonarQubeEnv('sonarqube') {
						sh "${scannerHome}/bin/sonar-scanner"
						sleep(10)
				}
				
		}
	}

	stage('Sonar-quality-gate') {
	  		
		steps {

			waitForQualityGate abortPipeline: true
				
				
		}
	}	
**/     
    stage('Build') {
        
		steps {
			wrap([$class: 'BuildUser']) {
      		//send build started notifications
            slackSend (color: '#FFFF00', message: "${server} ${BUILD_USER_ID} BUILD STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
			}
              
 			sh "sh ${WORKSPACE}/devops/ci_server/user_home/build_git.sh ${env.BRANCH_NAME}-${env.BUILD_ID}"
            echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
        }  
    }
    
    stage('Deploy Test'){
    
		environment {
		
 			def version = sh(script: "grep -v '[^ 0-9]+' /tmp/version.${env.BRANCH_NAME}-${env.BUILD_ID}" , , returnStdout: true).trim()
			def server = "${params.environment}"
		}
		
			when {
        
            allOf {
                expression { USER_ID ==~ /(duggalk|akagarwal|pusingh|srajvanshi|ssgeorge|ragarwal|vsingh|jmohammad)/ }
				expression { environment ==~ /(pls-aps-t03)/ }
				expression { environment != 'None' }
                }
            }

				
                
                
            
		steps {
			  wrap([$class: 'BuildUser']) {
			  // send build started notifications
              slackSend (color: '#FFFF00', message: "${server} ${BUILD_USER_ID} DEPLOYMENT STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
			  }

			  sh '''
			      echo ${server}
				  echo Assembly version number: $ASSEMBLY
				  for i in ${server}; do rsync -avh ${WORKSPACE}/devops/deployments/$i intershop1@$i:/opt/intershop/install/new_workspace; done
				  ssh -t intershop1@${server} 					 echo "############ START DEPLOYMENT ############"
				   ssh -t intershop1@${server} ASSEMBLY=${version} sh /opt/intershop/install/new_workspace/${server}/deploy.sh
				  ssh -t intershop1@${server} 					 echo "############ END OF SFS DEPLOYMENT ############"
							
						'''
				script {
 					currentBuild.description = "Nexus Version: ${version} \n Environment: ${server} \n Branch: ${BRANCH_NAME}"
					
 					}
 					
		    
		}
    }

	stage('Deploy Test 2'){
    
		environment {
		
 			def version = sh(script: "grep -v '[^ 0-9]+' /tmp/version.${env.BRANCH_NAME}-${env.BUILD_ID}" , , returnStdout: true).trim()
			def server = "${params.environment}"
		}
		
			when {
        
            allOf {
                expression { USER_ID ==~ /(duggalk|akagarwal|pusingh|srajvanshi|ssgeorge|ragarwal|vsingh|jmohammad)/ }
				expression { environment ==~ /(pls-aps-t04)/ }
				expression { environment != 'None' }
                }
            }

				
                
                
            
		steps {
			  wrap([$class: 'BuildUser']) {
			  // send build started notifications
              slackSend (color: '#FFFF00', message: "${server} ${BUILD_USER_ID} DEPLOYMENT STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
			  }

			  sh '''
			      echo ${server}
				  echo Assembly version number: $ASSEMBLY
				  for i in ${server}; do rsync -avh ${WORKSPACE}/devops/deployments/$i intershop1@$i:/opt/intershop/install/new_workspace; done
				  ssh -t intershop1@${server} 					 echo "############ START DEPLOYMENT ############"
				   ssh -t intershop1@${server} ASSEMBLY=${version} sh /opt/intershop/install/new_workspace/${server}/deploy.sh
				  ssh -t intershop1@${server} 					 echo "############ END OF SFS DEPLOYMENT ############"
							
						'''
				script {
 					currentBuild.description = "Nexus Version: ${version} \n Environment: ${server} \n Branch: ${BRANCH_NAME}"
					
 					}
 					
		    
		}
    }

	stage('Deploy AWS Test'){
    
		environment {
		
 			def version = sh(script: "grep -v '[^ 0-9]+' /tmp/version.${env.BRANCH_NAME}-${env.BUILD_ID}" , , returnStdout: true).trim()
			def server = "${params.environment}"
		}
		
			when {
        
            allOf {
                expression { USER_ID ==~ /(admin|duggalk|akagarwal|pusingh|srajvanshi|ssgeorge|ragarwal|vsingh|jmohammad)/ }
				expression { environment ==~ /(pls-aps-t05)/ }
				expression { environment != 'None' }
                }
            }

				
                
                
            
		steps {
			  wrap([$class: 'BuildUser']) {
			  // send build started notifications
              slackSend (color: '#FFFF00', message: "${server} ${BUILD_USER_ID} DEPLOYMENT STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
			  }

			  sh '''
			      echo ${server}
				  echo Assembly version number: $ASSEMBLY
				  for i in ${server}; do rsync -avh ${WORKSPACE}/devops/deployments/$i intershop1@$i:/opt/intershop/install/new_workspace; done
				  ssh -t intershop1@${server} 					 echo "############ START DEPLOYMENT ############"
				   ssh -t intershop1@${server} ASSEMBLY=${version} sh /opt/intershop/install/new_workspace/${server}/deploy.sh
				  ssh -t intershop1@${server} 					 echo "############ END OF SFS DEPLOYMENT ############"
							
						'''
				script {
 					currentBuild.description = "Nexus Version: ${version} \n Environment: ${server} \n Branch: ${BRANCH_NAME}"
					
 					}
 					
		    
		}
    }

	stage('Deploy AWS Test 2'){
    
		environment {
		
 			def version = sh(script: "grep -v '[^ 0-9]+' /tmp/version.${env.BRANCH_NAME}-${env.BUILD_ID}" , , returnStdout: true).trim()
			def server = "${params.environment}"
		}
		
			when {
        
            allOf {
                expression { USER_ID ==~ /(admin|duggalk|akagarwal|pusingh|srajvanshi|ssgeorge|ragarwal|vsingh|jmohammad)/ }
				expression { environment ==~ /(pls-aps-t06)/ }
				expression { environment != 'None' }
                }
            }

				
                
                
            
		steps {
			  wrap([$class: 'BuildUser']) {
			  // send build started notifications
              slackSend (color: '#FFFF00', message: "${server} ${BUILD_USER_ID} DEPLOYMENT STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
			  }

			  sh '''
			      echo ${server}
				  echo Assembly version number: $ASSEMBLY
				  for i in ${server}; do rsync -avh ${WORKSPACE}/devops/deployments/$i intershop1@$i:/opt/intershop/install/new_workspace; done
				  ssh -t intershop1@${server} 					 echo "############ START DEPLOYMENT ############"
				   ssh -t intershop1@${server} ASSEMBLY=${version} sh /opt/intershop/install/new_workspace/${server}/deploy.sh
				  ssh -t intershop1@${server} 					 echo "############ END OF SFS DEPLOYMENT ############"
							
						'''
				script {
 					currentBuild.description = "Nexus Version: ${version} \n Environment: ${server} \n Branch: ${BRANCH_NAME}"
					
 					}
 					
		    
		}
    }
	
    stage('Deploy ACC'){
    
		environment {
		
 			def version = sh(script: "grep -v '[^ 0-9]+' /tmp/version.${env.BRANCH_NAME}-${env.BUILD_ID}" , , returnStdout: true).trim()
			def server = "${params.environment}"
		}
		
			when {
        
            allOf {
                expression { USER_ID ==~ /(ssgeorge|ragarwal|vsingh|akagarwal|jmohammad)/ }
				expression { environment ==~ /(pls-aps-p01)/ }
				expression { environment != 'None' }
                }
            }

				
                
                
            
		steps {
			  wrap([$class: 'BuildUser']) {
			  // send build started notifications
              slackSend (color: '#FFFF00', message: "${server} ${BUILD_USER_ID} DEPLOYMENT STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
			  }

			  sh '''
			      echo ${server}
				  echo Assembly version number: $ASSEMBLY
				  for i in ${server}; do rsync -avh ${WORKSPACE}/devops/deployments/$i intershop1@$i:/opt/intershop/install/new_workspace --delete-before ; done
				  ssh -t intershop1@${server} 					 echo "############ START DEPLOYMENT ############"
				   ssh -t intershop1@${server} ASSEMBLY=${version} sh /opt/intershop/install/new_workspace/${server}/deploy.sh
				  ssh -t intershop1@${server} 					 echo "############ END OF SFS DEPLOYMENT ############"
							
						'''
				script {
 					currentBuild.description = "Nexus Version: ${version} \n Environment: ${server} \n Branch: ${BRANCH_NAME}"
					
 					}
 					
		    
		}
    }
    
    stage('Invalidate Pagecache') {
        environment {
	  		def server = "${params.environment}"
        }
		when {
		    allOf {
		        
                expression { InvalidatePagecache == 'true' }
		    }
                    
        }
		steps {
				
			sh 'ssh -t intershop1@${server} ASSEMBLY=${nexusversion} sh /opt/intershop/install/new_workspace/${server}/invalidate_pagecache.sh -o soft'
        
	    }
	}
    
      stage('Startup Status') {
        environment {
	  		def server = "${params.environment}"
        }
        when {
                expression { environment != 'None' }
            
            }
		steps {

			sh "sh statuscodecheck.sh ${server}" 
				
				
		}
	}
  }

   post {
      
      success {
          wrap([$class: 'BuildUser']){
          slackSend (color: '#00FF00', message: "${server} ${BUILD_USER_ID} SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
          
          
                mail to: 'himag@pembridgesoft.ca', from: 'systeembeheer@pembridgesoft.ca',
                subject: "PLUS Pipeline: ${env.JOB_NAME} - +Success", 
                body: "Job Failed - \"${env.JOB_NAME}\" build: ${env.BUILD_NUMBER}\n\nView the log at:\n ${env.BUILD_URL}\n\nBlue Ocean:\n${env.RUN_DISPLAY_URL}" 
	 	 }
        }
        failure {
		  wrap([$class: 'BuildUser']) {
          slackSend (color: '#FF0000', message: "${server} ${BUILD_USER_ID} FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
		
            mail to: 'himag@pembridgesoft.ca', from: 'systeembeheer@pembridgesoft.ca',
            subject: "Example Build: ${env.JOB_NAME} - Failed", 
            body: "Job Failed - \"${env.JOB_NAME}\" build: ${env.BUILD_NUMBER}\n\nView the log at:\n ${env.BUILD_URL}\n\nBlue Ocean:\n${env.RUN_DISPLAY_URL}"
		  }
        }
    }

  }
 

