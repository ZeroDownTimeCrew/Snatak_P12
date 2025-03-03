pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'echo "Repository Checked Out: $(git rev-parse --abbrev-ref HEAD)"'
            }
        }
        stage('PR Validation') {
            steps {
                script {
                    def branchName = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    
                    if (!branchName.startsWith('feature-')) {
                        error("❌ PR validation failed: Branch name must start with 'feature-'")
                    } else {
                        echo "✅ PR validation passed: Branch name is correct."
                    }
                }
            }
        }
        stage('Jenkins Approval') {
            steps {
                echo "✅ Jenkins has approved the PR. Ready for review."
            }
        }
    }
}
