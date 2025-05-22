pipeline {
    agent any
    stages {
        stage('Build') {          
            steps {
                script {
                    // Set display name: e.g., feature-1.14
                    currentBuild.displayName = "${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
                }  
                echo 'Building the application...'
            }
        }
        stage('Scans') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'main'
                }
            }            
            parallel {
                stage('TruffleHog Scan') {
                    steps {
                        echo 'Running BlackDuck Scan...'
                        // Insert TruffleHog scan command here
                    }
                }                
                stage('Checkmarx Scan') {
                    steps {
                        echo 'Running Checkmarx Scan...'
                        // Insert Checkmarx scan command here
                    }
                }
                stage('BlackDuck Scan') {
                    steps {
                        echo 'Running BlackDuck Scan...'
                        // Insert BlackDuck scan command here
                    }
                }
            }
        }        
        stage('Tests') {
           parallel {
               stage('Unit Tests') {
                   steps {
                       echo 'Running Unit tests...'
                       // sh 'pytest tests/unit' or mvn test, etc.
                   }
               }
                stage('Integration Tests') {
                    steps {
                        echo 'Running integration tests...'
                        // sh 'pytest tests/integration' or docker-compose test
                    }
                }

                stage('API Tests') {
                    steps {
                        echo 'Running API tests...'
                        // sh 'newman run postman_collection.json'
                    }
                }
            }               
        }
        stage('Publish') {
            steps {
                echo 'Publishing artifacts to Artifactory...'  
                // Insert Artifactory publish command here
            }
        }   
        stage('DEVDeploy') {
            steps {
                script {
                    def userInput = input(
                        id: 'DeployApproval', message: 'Deploy to DEV?', ok: 'Approve',
                        parameters: [
                            string(name: 'Deployer', defaultValue: '', description: 'Enter your name'),
                            choice(name: 'Environment', choices: ['DEV', 'QA', 'PROD'], description: 'Choose environment')
                        ]
                    )
                    echo "Approved by: ${userInput['Deployer']} for environment: ${userInput['Environment']}"
                }                
                echo 'Deploying the application...'
            }
        }
        stage('QADeploy') {
            steps {
                script {
                    def userInput = input(
                        id: 'DeployApproval', message: 'Deploy to QA?', ok: 'Approve',
                        parameters: [
                            string(name: 'Deployer', defaultValue: '', description: 'Enter your name'),
                            choice(name: 'Environment', choices: ['DEV', 'QA', 'STAGE', 'PROD'], description: 'Choose environment')
                        ]
                    )
                    echo "Approved by: ${userInput['Deployer']} for environment: ${userInput['Environment']}"
                }                
                echo 'Deploying the application...'
            }
        }
        stage('PRODDeploy') {
            steps {
                script {
                    def userInput = input(
                        id: 'DeployApproval', message: 'Deploy to Production?', ok: 'Approve',
                        parameters: [
                            string(name: 'Deployer', defaultValue: '', description: 'Enter your name'),
                            choice(name: 'Environment', choices: ['DEV', 'QA', 'STAGE', 'PROD'], description: 'Choose environment')
                        ]
                    )
                    echo "Approved by: ${userInput['Deployer']} for environment: ${userInput['Environment']}"
                }                
                echo 'Deploying the application...'
            }
        }                      
    }
    post {
        success {
            echo 'Pipeline completed successfully.'
            // Slack/email notification
        }
        failure {
            echo 'Pipeline failed.'
            // Slack/email notification with logs
        }
    }
}
