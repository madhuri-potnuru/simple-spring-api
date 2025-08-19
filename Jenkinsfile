pipeline {
    agent any

    tools {
        jdk 'jdk-21'
        maven 'apache-maven-3.9.10'
    }

    environment {
        // SonarQube details
        SONARQUBE_SERVER = 'sonarcloud'
        SONAR_PROJECT_KEY = 'madhuri-potnuru_simple-spring-api'
        SONAR_PROJECT_NAME = 'simple-spring-api'
        SONAR_ORGANIZATION = 'madhuri-potnuru'

        // Docker / Deploy details
        APP_NAME = 'simple-spring-api'
        DOCKER_IMAGE = "madhuri-potnoru/${APP_NAME}"
        CONTAINER_NAME = 'simple-spring-api'
        APP_PORT = '9595'
        DOCKERHUB_CREDENTIALS = 'docker-credentials'
        HOST_PORT_MAPPING = '9595:9595'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/madhuri-potnuru/simple-spring-api'
            }
        }

        stage('Build') {
            steps {
                echo 'Building with Maven...'
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('sonarcloud') {
            withCredentials([string(credentialsId: 'login', variable: 'SONAR_TOKEN')]) {
                bat "mvn sonar:sonar " +
                    "-Dsonar.projectKey=${SONAR_PROJECT_KEY} " +
                    "-Dsonar.organization=${SONAR_ORGANIZATION} " +
                    "-Dsonar.host.url=https://sonarcloud.io " +
                    "-Dsonar.login=${SONAR_TOKEN}"
            }
        }
    }
}


        stage('Deploy') {
            steps {
                echo 'Deploying to a server...'
                echo 'Deployment successful!'
            }
        }
    }
}
