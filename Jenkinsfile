pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn -B -DskipTests clean package sonar:sonar'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Install') {
            steps {
                sh 'mvn install'
            }
        }
    }
}