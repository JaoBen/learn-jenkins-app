pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'nfp_LrGFNrZ4KCrD72A7ifcSDyeZJYAqtBaHf804'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo "Checking required files..."
                sh '''
                    test -f index.html || (echo "Missing index.html" && exit 1)
                    test -f netlify/functions/quote.js || (echo "Missing quote function" && exit 1)
                    echo "Build check passed."
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
                echo "Testing quote function load..."
                sh '''
                    node -e "require('./netlify/functions/quote.js'); console.log('Function loaded successfully')"
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo "Deploying to Netlify..."
                sh '''
                    npm install netlify-cli
                    npx netlify deploy --prod --dir=build --auth=$NETLIFY_AUTH_TOKEN --site=$NETLIFY_SITE_ID
                '''
            }
        }

        stage('Post Deploy') {
            steps {
                echo "Deployment complete! Your app is live."
            }
        }
    }

    post {
        success {
            echo "CI/CD pipeline finished successfully."
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
