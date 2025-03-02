pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-token-id') // Store GitHub token in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'make build'  // Replace with actual build command
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'make test'  // Replace with actual test command
            }
        }

        stage('Lint') {
            steps {
                echo 'Checking code quality...'
                sh 'make lint'  // Replace with actual linting command
            }
        }

        stage('Approval Check') {
            steps {
                script {
                    def prId = env.CHANGE_ID
                    if (!prId) {
                        echo "Not a pull request build. Skipping approval check."
                        return
                    }

                    def reviewers = sh(script: """
                        gh pr view $prId --repo your-org/your-repo --json reviews --jq '.reviews | map(select(.state=="APPROVED")) | length'
                    """, returnStdout: true).trim()

                    echo "Number of approvals: ${reviewers}"
                    if (reviewers.toInteger() < 2) {
                        error "At least 2 reviewers must approve this PR before merging!"
                    }
                }
            }
        }
    }
}
