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
        echo '✅ Build success!'
        emailext (
            subject: "✅ Jenkins Build Successful",
            body: "The build ${env.JOB_NAME} #${env.BUILD_NUMBER} finished successfully.",
            to: "your_email@gmail.com"
        )
    }
    failure {
        echo '❌ Build failed!'
        emailext (
            subject: "❌ Jenkins Build Failed",
            body: "The build ${env.JOB_NAME} #${env.BUILD_NUMBER} failed.\nCheck logs in Jenkins.",
            to: "your_email@gmail.com"
        )
    }
}
