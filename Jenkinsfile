pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
		      deleteDir()	
                git branch: 'main', url: 'https://github.com/Chintan358/java_pipeline.git'
            }
        }

        stage('Build JAR with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                  sh 'pwd && ls -la'
                sh 'docker build -t javaapp_demo:latest .'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d --build'
            }
        }

        stage('Verify Application') {
            steps {
                sh 'sleep 10 && curl -f http://localhost:8000 || exit 1'
            }
        }
    }

    post {
        success {
            echo '✅ JavaApp deployed successfully on AWS Ubuntu!'
        }
        failure {
            echo '❌ Pipeline failed! Check logs.'
        }
    }
}
