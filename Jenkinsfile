pipeline {
    agent any

    environment {
        NODE_VERSION = "18.19.1"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                echo "🧹 Cleaning Jenkins Workspace..."
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                echo "📦 Cloning GitHub Repository..."
                git branch: 'main',
                    url: 'https://github.com/lallanborasi12/jenkinstest.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Setup Node.js') {
            steps {
                echo "⚙️ Installing Node.js if not installed..."
                sh '''
                if ! command -v node &> /dev/null
                then
                  echo "Node.js not found. Installing..."
                  curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
                  sudo apt-get install -y nodejs
                fi
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies & Build') {
            steps {
                echo "📦 Installing dependencies and building project..."
                sh '''
                npm cache clean --force
                rm -rf node_modules package-lock.json
                npm install
                npm install react-scripts --save
                npm run build
                '''
            }
        }

        stage('Deploy with PM2') {
            steps {
                echo "🚀 Deploying React App using PM2..."
                sh '''
                sudo npm install -g pm2
                pm2 delete all || true
                pm2 serve build 3000 --spa --name "react-app"
                pm2 save
                pm2 list
                '''
            }
        }

        stage('Deploy to /var/www/html') {
            steps {
                echo "📂 Deploying build to /var/www/html..."
                sh '''
                sudo rm -rf /var/www/html/*
                sudo cp -r build/* /var/www/html/
                sudo chmod -R 755 /var/www/html
                echo "✅ Deployment Complete! App available at /var/www/html"
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Deployment Successful!"
        }
        failure {
            echo "❌ Build Failed. Please check the logs."
        }
    }
}
