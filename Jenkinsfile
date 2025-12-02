pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token');
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/YordanovPetar/DevOpsTrainings.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-netlify-agent .'
            }
        }

        stage('Deploy to Netlify') {
            steps {
                sh '''
                docker run --rm \
                  -e NETLIFY_AUTH_TOKEN=$NETLIFY_AUTH_TOKEN \
                  -v $PWD:/deploys \
                  jenkins-netlify-agent \
                  sh -c "netlify deploy --prod --dir=/deploys --auth=$NETLIFY_AUTH_TOKEN --site=4862e94e-79f5-4d87-b6e2-dfe42874daa0"
                '''
            }
        }
    }
}