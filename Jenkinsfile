cplxStrng = "";
nameSpaceFolder = "";
deplymentsYamlFolder = ""

pipeline {
    //agent any //Use this for default
    agent{
	    node{
	    	label 'cd-jenkins'
	    }
    }
    environment {
		CREDENTIALS_ID = "${CREDENTIALS_ID}"
        PROJECT_FOLDER = "${PROJECT_FOLDER}"
        PROJECT_ID = "${PROJECT_ID}"
        CLUSTER_NAME = "${CLUSTER_NAME}"
        LOCATION = "${LOCATION}"

    }
    stages {
        stage("Verify variables") {
            steps {
                echo "CREDENTIALS_ID: ${params.CREDENTIALS_ID}; PROJECT_FOLDER: ${params.PROJECT_FOLDER}"
                echo "CREDENTIALS_ID: ${CREDENTIALS_ID}; PROJECT_FOLDER: ${PROJECT_FOLDER}"
            }
        }
        stage("Checkout code") {
            steps {
                echo "Start checkout"
                checkout scm
            }
        }
        
        stage('Creating name space') {
            steps{
                sh "ls ${PROJECT_FOLDER}/"

                script {
                    nameSpaceFolder = PROJECT_FOLDER + '/' + PROJECT_FOLDER+ '-namespace.yaml'
                }

                echo "${PROJECT_FOLDER} ${PROJECT_ID} ${CLUSTER_NAME} ${LOCATION}"
                echo "nameSpaceFolder: ${nameSpaceFolder}"
                
                step([$class: 'KubernetesEngineBuilder', projectId: PROJECT_ID, clusterName: CLUSTER_NAME, location: LOCATION, manifestPattern: nameSpaceFolder, credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])

            }
        }
        /*
        stage('Applying all yaml to GKE') {
            steps{
                echo "${PROJECT_FOLDER} ${PROJECT_ID} ${CLUSTER_NAME} ${LOCATION}"

                script {
                    deplymentsYamlFolder = PROJECT_FOLDER + '/'
                }

                echo "deplymentsYamlFolder : ${deplymentsYamlFolder}"

                sh "ls ${deplymentsYamlFolder}"
                //step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: PROJECT_FOLDER, credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                step([$class: 'KubernetesEngineBuilder', projectId: PROJECT_ID, clusterName: CLUSTER_NAME, location: LOCATION, manifestPattern: deplymentsYamlFolder, credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])

            }
        }
        */
    }    
}