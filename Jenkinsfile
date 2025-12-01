pipeline {
    agent any
    
    options {
        // Notify GitHub about pipeline start & result
        githubNotify(context: 'jenkins-ci')
    }

    environment {
        // These come from Jenkins credentials binding
        AWS_ACCESS_KEY_I     = credentials('aws_access_key')
        AWS_SECRET_ACCESS_KEY = credentials('aws_secret_key')
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo "Cloning repo..."
                git branch: 'main',
                    url: 'https://github.com/skillup-terraform/terraform-pr.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh """
                    terraform init
                """
            }
        }

        stage('Terraform Plan 1 (PR Only)') {
            when {
                expression { env.CHANGE_ID != null }   // Only pull requests
            }
            steps {
                echo "Pull Request detected! PR Number: ${env.CHANGE_ID}"
                echo "Running terraform plan..."

                sh """
                    terraform plan
                """
            }
        }

        stage('Terraform Plan 2 (PR Only)') {
            when {
                expression { env.CHANGE_ID != null }   // Only pull requests
            }
            steps {
                echo "Pull Request detected! PR Number: ${env.CHANGE_ID}"
                echo "Running terraform plan..."

                sh """
                    terraform plan
                """
            }
        }

        stage('Terraform Apply (Main Branch Only)') {
            when {
                branch 'main'
                expression { env.CHANGE_ID == null }  // Not a PR, only normal main push
            }
            steps {
                echo "Main branch commit detected—running terraform apply..."

                sh """
                    terraform apply
                """
            }
        }
    }

post {
        success {
            githubNotify(context: 'jenkins-ci', status: 'SUCCESS', description: 'Build passed')
        }
        failure {
            githubNotify(context: 'jenkins-ci', status: 'FAILURE', description: 'Build failed')
        }
        aborted {
            githubNotify(context: 'jenkins-ci', status: 'ERROR', description: 'Build aborted')
        }
    }
    
}
