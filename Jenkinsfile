pipeline {
    agent any
    parameters {
        booleanParam(name: 'RUN_DEPLOY', defaultValue: true, description: 'Should we deploy?')
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Target environment') 
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building application...'
            }
        }
        stage('Test in Parallel') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        echo 'Running unit tests...'
                        sh 'sleep 5'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        echo 'Running integration tests...'
                        sh 'sleep 5'
                    }
                }
            }
        }

        stage('OS Matrix (Simulated)') {
            parallel {
                stage('Linux Sim') {
                    steps { echo 'Simulating tests on Linux...' }
                }
                stage('Windows Sim') {
                    steps { echo 'Simulating tests on Windows...' }
                }
            }
        }

        stage('Test') {
            steps {
                sh 'echo "All tests passed!" > results.txt'
                archiveArtifacts artifacts: 'results.txt', fingerprint: true
            }
        }
        stage('Approval') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    input "Do you want to proceed with deployment to '${params.ENV}'?"
                }
            }
        }
        stage('Deploy') {
            when {
                expression { return params.RUN_DEPLOY }
            }
            steps {
                echo "Deploying application to environment: ${params.ENV}"
            }
        }
    }
    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs!'
        }
        always {
            echo 'Pipeline completed (success or failure).'
        }
    }
}
