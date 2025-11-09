pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                sh '''
                    echo "=== Files in workspace ==="
                    ls -la
                    echo "=== Checking package.json ==="
                    cat package.json
                '''
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "=== Node Version ==="
                    node --version
                    npm --version
                    
                    echo "=== Installing Dependencies ==="
                    npm ci
                    
                    echo "=== Building Application ==="
                    npm run build
                    
                    echo "=== Build Complete ==="
                    ls -la build/
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "=== Running Tests ==="
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}