pipeline {
    agent { label 'java' }
    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf bus_booking'
                sh 'git clone https://github.com/tarundanda147/bus_booking.git'
            }
        }

        stage('build') {
            steps {
                script {
                    sh "$mvn clean install"
                }
            }
        }
        stage('Run JAR Locally') {
            steps {
                script {   
                    sh "java -jar target/bus-booking-app-1.0-SNAPSHOT.jar"
                }
            }
        }

        stage('Deploy JAR to Tomcat') {
            steps {
                script {
                    // Copy JAR to Tomcat server
                    sh "scp target/bus-booking-app-1.0-SNAPSHOT.jar root@172.31.47.152:/opt/apache-tomcat-9.0.85/webapps/"
                    sh "ssh root@$172.31.47.152"
                    echo "Application deployed and Tomcat restarted"
                }
            }
        }
    }

    post {
        success {
            echo "Build, Run, and Deployment to Tomcat successful!"
        }
        failure {
            echo "Build, Run, and Deployment to Tomcat failed!"
        }
    }
}
