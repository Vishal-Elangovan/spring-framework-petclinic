pipeline {
    agent any

    tools {
        maven 'Maven'  // Make sure Maven tool is configured in Jenkins (Manage Jenkins > Global Tool Config)
    }

    environment {
        SONARQUBE_URL = 'http://43.204.37.49:9000'
        SONAR_TOKEN = credentials('sqa_6152e5c00d32d3ad1a0cac3bbed102b278ff8175')  // Set this in Jenkins Credentials
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Vishal-Elangovan/spring-framework-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh "mvn sonar:sonar -Dsonar.projectKey=petclinic -Dsonar.login=$SONAR_TOKEN"
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'cp target/*.war /home/ubuntu/apache-tomcat-10.1.43/webapps/'  // Adjust path to your Tomcat instance
            }
        }
    }
}
