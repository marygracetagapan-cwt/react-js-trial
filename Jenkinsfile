#!/usr/bin/env groovy

pipeline {
    agent any
	
	parameters {
		string(name: 'SOURCE_BRANCH', defaultValue: 'master', description: 'Branch to build and deploy')
		string(name: 'REGION_NAME', defaultValue: 'us-west-2', description: 'AWS Region')
		string(name: 'TAG_VERSION', defaultValue: '0.0.1-SNAPSHOT', description: 'Docker tag version')
		
		string(name: 'PROJECT_NAME', defaultValue: 'bpg-ng-web-ui', description: 'Project-name')
		
		booleanParam(name: 'DEPLOY_TO_DEV', defaultValue: true, description: 'Deploy to Dev Environment?')
		booleanParam(name: 'DEPLOY_TO_TEST', defaultValue: false, description: 'Deploy to Test Environment?')
		booleanParam(name: 'DEPLOY_TO_INT', defaultValue: false, description: 'Deploy to Test Environment?')
		booleanParam(name: 'DEPLOY_TO_PREPROD', defaultValue: false, description: 'Deploy to Preprod Environment?')
		booleanParam(name: 'DEPLOY_TO_PROD', defaultValue: false, description: 'Deploy to Production Environment?')
	}
    	stages {
	
        stage ('Prepare Build') {
			when {
				expression {
				  return !params.SKIP_BUILD
				  }
			}
			steps {
					echo 'Pulling from Github  Stage'
					git branch: 'master', credentialsId: 'SSO-SAML', url: 'https://github.com/cwt-dev/bpg-ng-web-ui.git'
				}
			}

        stage ('Build Stage') {
			when {
				expression {
				  return !params.SKIP_BUILD
				  }
			}
            steps {

            	sh 'echo $PATH'
             	echo 'Building UI'
				sh "npm run build"
			   
                }
		}		
        stage ('Deployment to DEV') {
		
			when {
				expression {
					return params.DEPLOY_TO_DEV
			  }
			}
			environment {
				ENVIRONMENT = 'dev'
			}
			steps {
			echo 'Deploying...'
			script {
				deployToS3Bucket()
			}
				
				
            }
        }
		stage ('Deployment to TEST') {
		
			when {
				expression {
					return params.DEPLOY_TO_TEST
			  }
			}
			environment {
				ENVIRONMENT = 'test'
			}
			steps {
			echo 'Deploying...'
			script {
			
				deployToS3Bucket()
			}
				
				
            }
        }
		stage ('Deployment to INT') {
		
			when {
				expression {
					return params.DEPLOY_TO_INT
			  }
			}
			environment {
				ENVIRONMENT = 'int'
			}
			steps {
			echo 'Deploying...'
			script {
				
				deployToS3Bucket()
			}
				
				
            }
        }
		stage ('Deployment to Staging') {
		
			when {
				expression {
					return params.DEPLOY_TO_STAGING
			  }
			}
			environment {
				ENVIRONMENT = 'staging'
			}
			steps {
			echo 'Deploying...'
			script {
				
				deployToS3Bucket()
			}
				
				
            }
        }
		stage ('Deployment to Prod') {
		
			when {
				expression {
					return params.DEPLOY_TO_PROD
			  }
			}
			environment {
				ENVIRONMENT = 'prod'
			}
			steps {
			echo 'Deploying...'
			script {
				
				deployToS3Bucket()
			}
				
				
            }
        }
    }
}

def createS3Bucket(){
	echo 'Creating S3 Bucket'
	sh 'aws s3api create-bucket --bucket ${PROJECT_NAME} --region ${REGION_NAME}'
}

def deployToS3Bucket(){
	echo 'Deploying to S3 Bucket'
	sh 'aws s3 cp ${PROJECT_NAME}/build/ s3://${PROJECT_NAME}/ --recursive'
}