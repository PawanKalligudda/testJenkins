pipeline {
    agent { 
        docker { 
            image 'python:3.11'
            args '-u root:root'
        } 
    }
    stages {
        stage('Testing & Source Scans') {
            when {
                anyOf {
                    branch 'main'
                    changeRequest() // triggers for PRs
                }
            }
            parallel {
                stage('Run Test Script') {
                    steps {
                        sh '''
                            echo "Starting backend tests..."
                            echo "backend test is running"
                            sleep 300
                            echo "backedn test finished"
                        '''
                    }
                }

                stage('Run Trivy Scan') {
                    steps {
                        sh '''
                           sh '''
                            echo "Starting trivy scans..."
                            echo "Trivy scans are running"
                            sleep 300
                            echo "Trivy scans finished"
                        '''
                        '''
                    }
                }
                stage('Run Detect Secrets') {
                    steps {
                        sh '''
                            echo "Installing detect-secrets..."
                            pip install detect-secrets
                            sleep 300
                            echo "Running detect-secrets scan..."
                            # detect-secrets scan > $output_file  # Uncomment when read
                        '''
                    }
                }
                stage('Run SonarQube') {
                    steps {
                        sh '''
                            echo "Installing SonarQube scanner dependencies..."
                            dnf update -qy && dnf install -y wget git unzip openjdk-17-jdk

                            export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
                            export PATH=$JAVA_HOME/bin:$PATH
                            java -version

                            echo "Running SonarQube analysis..."
                            # sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=.  # Uncomment when ready
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()
        }
    }
}
