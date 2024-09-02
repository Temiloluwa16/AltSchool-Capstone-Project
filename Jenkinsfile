pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = "eu-west-2"
    }
    parameters{
        choice(name: 'ENVIRONMENT', choices: ['create', 'destroy'], description: 'create and destroy cluster with one click')
    }
    stages {
     
        stage("Deploy prometheus") {
             when {
                expression { params.ENVIRONMENT == 'create' }
            }
            steps {
                script {
                    dir('kubernetes/prometheus-helm') {
                        sh "aws eks --region eu-west-2 update-kubeconfig --name hr-dev-eks-demo"
                        sh "terraform init"
                        sh "terraform apply -auto-approve"
                    }
                }
            }
        }

