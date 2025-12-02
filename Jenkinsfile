pipeline {
    agent any
    
    environment {
        // These come from Jenkins credentials binding
        AWS_ACCESS_KEY_ID     = "asdfffffffffffffffffff"
        AWS_SECRET_ACCESS_KEY = "asdfasdff"
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
                    echo "terraform init"
            }
        }

        stage('Terraform Plan 1 (PR Only)') {
            when {
                expression { env.CHANGE_ID != null }   // Only pull requests
            }
            steps {
                echo "Pull Request detected! PR Number: ${env.CHANGE_ID}"
                echo "Running terraform plan..."
            }
        }

        stage('Terraform Plan 2 (PR Only)') {
            when {
                expression { env.CHANGE_ID != null }   // Only pull requests
            }
            steps {
                echo "Pull Request detected! PR Number: ${env.CHANGE_ID}"
                echo "Running terraform plan..."

            }
        }

        stage('Terraform Apply (Main Branch Only)') {
            when {
                branch 'main'
                expression { env.CHANGE_ID == null }  // Not a PR, only normal main push
            }
            steps {
                echo "Main branch commit detected—running terraform apply..."
            }
        }
    }
    
}



