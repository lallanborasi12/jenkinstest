pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning Jenkins Workspace'
                deleteDir()
            }
        }

        stage('Clone Repo') {
            steps {
                echo 'Cloning Repository...'
                git branch: 'master', url: 'https://github.com/lallanborasi12/newrepo.git'
            }
        }

        stage('Install Dependencies & Build') {
            steps {
                echo 'Installing dependencies and building...'
                sh '''
                    cd $WORKSPACE
                    npm install
                    npm run build
                '''
            }
        }

        stage('Deploy with PM2') {
            steps {
                echo 'Deploying app using PM2...'
                sh '''
                    APP_DIR=/var/www/html4/reactjs-app

                    # Copy build files to deployment directory
                    sudo mkdir -p $APP_DIR
                    sudo rsync -av --delete --exclude=.git --exclude=node_modules ./ $APP_DIR

                    cd $APP_DIR

                    # Ensure dependencies
                    npm install

                    # Start with PM2 (replace if already running)
                    
                  

                    # Save PM2 process list and enable on reboot
                    
                '''
            }
        }
    }
}
