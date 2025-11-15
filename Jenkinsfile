pipeline {
    agent { label 'aws' }

    tools {
        maven 'Maven3'
        jdk 'jdk25'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/cbvl33/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying application...'
                sh '''
                    JAR_FILE=$(ls target/*.jar | head -n 1)
                    cp $JAR_FILE /home/ubuntu/deploy/app.jar
                    nohup java -jar /home/ubuntu/deploy/app.jar > /home/ubuntu/deploy/app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build and tests completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
