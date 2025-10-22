pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://ec2-13-232-207-52.ap-south-1.compute.amazonaws.com:8082/manager/text'
        TOMCAT_USER = 'Krishnacloud'
        TOMCAT_PASSWORD = 'Temp@#12312'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project with Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR to Tomcat...'
                sh """
                curl --fail --upload-file target/my-web-app-1.0-SNAPSHOT.war \
                --user $TOMCAT_USER:$TOMCAT_PASSWORD \
                $TOMCAT_URL/my-web-app.war
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check logs for errors.'
             }
    }
}
