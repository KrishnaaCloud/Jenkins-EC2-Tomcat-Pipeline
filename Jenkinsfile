pipeline {
    agent any
    environment {
        TOMCAT_URL = 'http://ec2-13-232-207-52.ap-south-1.compute.amazonaws.com:8080/manager/text'
        TOMCAT_USER = 'Krishnacloud'
        TOMCAT_PASSWORD = 'Temp@#12312'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat(url: env.TOMCAT_URL, credentialsId: 'tomcat-creds')], contextPath: '/', war: '**/*.war'
            }
        }
    }
}
