pipeline {
    agent{
        node{
            label 'cd-jenkins'
		}
	}
  parameters{
      string (defaultValue: 'staging',description: '',name : 'ENV_TYPE')
      string (defaultValue: 'test-cluster',description: '',name : 'CUSTOMER_NAME')
      string (defaultValue: 'us-central1-c',description: '',name : 'LOCATION')
  }
  environment {
        //PRODUCT_NAME= "${PRODUCT_NAME}"
        //VERSION = "${VERSION}"
        LOCATION = "${LOCATION}"
        CLUSTER_NAME = "${CUSTOMER_NAME}-${ENV_TYPE}"
        CUSTOMER_NAME = "${CUSTOMER_NAME}"
        DNS_NAME = "${ENV_TYPE != "production" ? "${CUSTOMER_NAME}.${ENV_TYPE}.wavenetcloud.com" : "${CUSTOMER_NAME}.wavenetcloud.com"}"
        DNS_ZONE_NAME = "${ENV_TYPE != "production" ? "${CUSTOMER_NAME}-${ENV_TYPE}-wavenetcloud-com" : "${CUSTOMER_NAME}-wavenetcloud-com"}"
    }
    stages {
        /*
        stage("Checkout Deployment Scripts") {
            steps {
                git branch: 'feature/testing',
                credentialsId: 'kajan-bitbucket',
                url: "https://kajan-wn@bitbucket.org/global-wavenet/compose-deploy-yaml-files.git"
            }
        }
        */
        stage("Import Cluster Config"){
            steps{
                echo "Import kubeconfig entry for ${CLUSTER_NAME} -START"
                sh "gcloud container clusters get-credentials ${CLUSTER_NAME} --zone=${LOCATION}"
                echo "Import kubeconfig entry for ${CLUSTER_NAME} -END"
            }
        }

        ///////////////
        stage('Deploy Managed Certificate') {
            steps{
                echo "Deploy Managed Certificate to GKE -START"
                sh 'kubectl apply -f hello-world-service/managed-certificate.yaml;'
                echo "Deploy Managed Certificate to GKE -END"
            }
        }

        stage('Deploy Services') {
            steps{
                echo "Deploy Cloud Services to GKE -START"
                sh 'kubectl apply -f hello-world-service/managed-certificate-svc.yaml'
                echo "Deploy Cloud Services to GKE -END"
            }
        }

        stage('Apply Deployments') {
            steps{
                echo "Apply Deployments to GKE -START"
                sh 'kubectl apply -f hello-world-service/managed-certificate-deployment.yaml'
                echo "Apply Deployments to GKE -END"
            }
        }

        stage('Apply Ingress') {
            steps{
                echo "Apply Deployments to GKE -START"
                sh 'kubectl apply -f hello-world-service/managed-certificate-ingress.yaml'
                echo "Apply Deployments to GKE -END"
            }
        }
        //////////////

        /*
        stage("Prepare Deployment Scripts") {
            steps {
                echo "Replacing secret_name variable with a value in secret.yaml -START"
                    contentReplace(
                        configs: [
                            variablesReplaceConfig(
                                configs: [
                                    variablesReplaceItemConfig(
                                        name: 'SECRET_NAME',
                                        value: "$PRODUCT_NAME"
                                    )
                                ],
                                fileEncoding: 'UTF-8',
                                filePath: "secret.yaml",
                                variablesPrefix: '$',
                                variablesSuffix: ''
                                )]
                    )
                echo "Replacing secret_name variable with a value in secret.yaml -END"
                echo "Replacing secret_name variable with a value in deployments.yaml -START"
                    contentReplace(
                        configs: [
                            variablesReplaceConfig(
                                configs: [
                                    variablesReplaceItemConfig(
                                        name: 'SECRET_NAME',
                                        value: "$PRODUCT_NAME"
                                    )
                                ],
                                fileEncoding: 'UTF-8',
                                filePath: "${PRODUCT_NAME}/${VERSION}/cloud-services/cloud-services.yaml",
                                variablesPrefix: '$',
                                variablesSuffix: ''
                                )]
                    )
                    contentReplace(
                        configs: [
                            variablesReplaceConfig(
                                configs: [
                                    variablesReplaceItemConfig(
                                        name: 'SECRET_NAME',
                                        value: "$PRODUCT_NAME"
                                    )
                                ],
                                fileEncoding: 'UTF-8',
                                filePath: "${PRODUCT_NAME}/${VERSION}/deployments/deployments.yaml",
                                variablesPrefix: '$',
                                variablesSuffix: ''
                                )]
                    )
                echo "Replace secret_name variable with a value in deployments.yaml -END"
                echo "Replacing DNS in Services -START"
                    contentReplace(
                        configs: [
                            variablesReplaceConfig(
                                configs: [
                                    variablesReplaceItemConfig(
                                        name: 'DNS_NAME',
                                        value: "$DNS_NAME"
                                    )
                                ],
                                fileEncoding: 'UTF-8',
                                filePath: "${PRODUCT_NAME}/${VERSION}/deployments/deployments.yaml",
                                variablesPrefix: '$',
                                variablesSuffix: ''
                                )]
                    )
                    contentReplace(
                        configs: [
                            variablesReplaceConfig(
                                configs: [
                                    variablesReplaceItemConfig(
                                        name: 'DNS_NAME',
                                        value: "$DNS_NAME"
                                    )
                                ],
                                fileEncoding: 'UTF-8',
                                filePath: "${PRODUCT_NAME}/${VERSION}/services/services.yaml",
                                variablesPrefix: '$',
                                variablesSuffix: ''
                                )]
                    )
                echo "Replacing DNS in Services -END"
            }
        }

        stage("Apply Secrets"){
            steps{
                echo "Apply Secrets to GKE -START"
                sh 'kubectl apply -f secret.yaml'
                echo "Apply Secrets to GKE -END"
            }
        }

        stage('Deploy Cloud Services') {
            steps{
                echo "Deploy Cloud Services to GKE -START"
                sh 'set +e; kubectl apply -f ${PRODUCT_NAME}/${VERSION}/cloud-services/cloud-services.yaml; set -e;'
                echo "Deploy Cloud Services to GKE -END"
            }
        }

        stage('Verify Cloud Services') {
            steps{
                sh 'set +e; cd ${PRODUCT_NAME}/${VERSION}/cloud-services; chmod +x ./verify.sh; ./verify.sh; set -e;'
            }
        }

        stage('Deploy Services') {
            steps{
                echo "Deploy Services to GKE -START"
                sh 'kubectl apply -f ${PRODUCT_NAME}/${VERSION}/services/services.yaml'
                echo "Deploy Services to GKE -END"
            }
        }

        stage('Apply Deployments') {
            steps{
                echo "Apply Deployments to GKE -START"
                sh 'kubectl apply -f ${PRODUCT_NAME}/${VERSION}/deployments/deployments.yaml'
                echo "Apply Deployments to GKE -END"
            }
        }

        stage('Verify Deployments') {
            steps{
                sh 'set +e; cd ${PRODUCT_NAME}/${VERSION}/verification; chmod +x ./verify.sh; ./verify.sh; set -e;'
            }
        }
        */
    }
}
