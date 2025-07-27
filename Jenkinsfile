pipeline {
    agent  { label 'verisoft-2' }

    parameters {
        string(name: 'REPO_URL', defaultValue: 'https://github.com/dvora-wa/Automation.git', description: 'Repository URL')
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
    }

    environment {
        CENTRAL_BRANCH = 'main'
    }

    triggers {
        cron('30 5 * * 1\n0 14 * * *')
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository from ${params.REPO_URL} (branch: ${params.BRANCH_NAME})"
                timeout(time: 5, unit: 'MINUTES') {
                    git branch: "${params.BRANCH_NAME}", url: "${params.REPO_URL}"
                }
            }
        }

        stage('Compile') {
            steps {
                echo 'Starting compilation stage'
                timeout(time: 5, unit: 'MINUTES') {
                    sh 'mvn compile'
                }
                echo 'Compilation stage completed successfully'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Starting test stage'
                timeout(time: 5, unit: 'MINUTES') {
                    sh 'mvn test'
                }
                echo 'Test stage completed successfully'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
