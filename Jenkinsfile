pipeline {
    agent any

    tools {
        maven 'Maven'  // Make sure Maven tool is configured in Jenkins (Manage Jenkins > Global Tool Config)
    }

    environment {
        SONARQUBE_URL = 'http://65.2.168.222:9000'
        SONAR_TOKEN = credentials('sonar-token')  // Set this in Jenkins Credentials
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
