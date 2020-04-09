pipeline {
    
    agent {
        docker {
            image 'registry.hub.docker.com/google/cloud-sdk:alpine'
            args '-u root:root'
        }
    }

    environment {
        GOOGLE_PROJECT_ID = 'ics-belhocine-dev';
        CLUSTER_NAME = 'openshift';
        LOCATION = 'europe-west3-c';
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }
        /*stage('Install Kubectl') {
            steps {
                sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl"
                sh "chmod +x ./kubectl"
                sh "mv ./kubectl /usr/local/bin/kubectl"
            }
        }*/
        
        stage('Deployment') {
            steps {
                withCredentials([file(credentialsId: 'GCP_ics-belhocine-dev_service_account', variable: 'GOOGLE_SERVICE_ACCOUNT_KEY')]) {
                    sh """
                        echo "Connect to GKE"
                        gcloud auth activate-service-account --key-file=${GOOGLE_SERVICE_ACCOUNT_KEY};
                        echo "Connect to GKE";
                        gcloud container clusters get-credentials ${CLUSTER_NAME} --zone ${LOCATION} --project ${GOOGLE_PROJECT_ID}
                        echo "Authentication is done";

			gcloud components install kubectl
                        kubectl apply -f hello.yaml
                    """
                }      
            }
        }
     
    }
}
