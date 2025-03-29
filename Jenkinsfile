pipeline {
    agent any

    environment {
        NETLIFY_SITE_NAME = '5356c5a5-4829-4ea3-bdfe-76f0ddb77052' // 🔧 เปลี่ยนเป็นชื่อ site ที่ตั้งไว้ใน Netlify
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-slim'
                    reuseNode true
                }
            }
            steps {
                echo "✅ Checking required files..."
                sh '''
                    test -f public/index.html || (echo "❌ Missing index.html" && exit 1)
                    echo "✅ Build check passed."
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-slim'
                    reuseNode true
                }
            }
            steps {
                echo "🧪 Testing quote function load..."
                sh 'echo "⚠️ No test implemented yet"'
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-slim'
                    reuseNode true
                }
            }
            steps {
                echo "🚀 Deploying to Netlify..."
                sh '''
                    npm install
                    npm run build
                    npx netlify deploy --prod --dir=build --auth=$NETLIFY_AUTH_TOKEN --site=$NETLIFY_SITE_NAME
                '''
            }
        }

        stage('Post Deploy') {
            steps {
                echo "🎉 Deployment complete! Your app is live."
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD pipeline finished successfully."
        }
        failure {
            echo "❌ Pipeline failed. Check logs for details."
        }
    }
}
