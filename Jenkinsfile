pipeline {
    agent any
    stages {
        stage('Testing & Source Scans') {
            parallel {
                stage('Run Test Script') {
                    steps {
                        sh '''
                            docker run --rm -v $(pwd):/workspace -w /workspace python:3.11 bash -c "
                            echo "Starting backend tests..."
                            echo "backend test is running"
                            sleep 300
                            echo "backedn test finished"
                            "
                        '''
                    }
                }

                stage('Run Trivy Scan') {
                    steps {
                        sh '''
                            docker run --rm -v $(pwd):/workspace -w /workspace python:3.11 bash -c "
                            echo "Starting trivy scans..."
                            echo "Trivy scans are running"
                            sleep 300
                            echo "Trivy scans finished"
                            "
                        '''
                    }
                }
                stage('Run Detect Secrets') {
                    steps {
                        sh '''
                            docker run --rm -v $(pwd):/workspace -w /workspace python:3.11 bash -c "
                            echo "Installing detect-secrets..."
                            pip install detect-secrets
                            sleep 300
                            echo "Running detect-secrets scan..."
                            # detect-secrets scan > $output_file  # Uncomment when read
                            "
                        '''
                    }
                }
                stage('Run SonarQube') {
                    steps {
                        sh '''
                            docker run --rm -v $(pwd):/workspace -w /workspace python:3.11 bash -c "
                            echo "Installing SonarQube scanner dependencies..."
                            dnf update -qy && dnf install -y wget git unzip openjdk-17-jdk

                            export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
                            export PATH=$JAVA_HOME/bin:$PATH
                            java -version

                            echo "Running SonarQube analysis..."
                            # sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=.  # Uncomment when ready
                            "
                        '''
                    }
                }
                stage('Detect Secrets') {
                    steps {
                        sh '''
                            docker run --rm -v $(pwd):/workspace -w /workspace python:3.11 bash -c "
                            pip install detect-secrets
                            #detect-secrets scan > detect-secrets-results.json
                            "
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
