pipeline {
    agent any

    stages {
        stage('Build') {
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