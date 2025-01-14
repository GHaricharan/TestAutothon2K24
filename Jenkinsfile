pipeline {
    agent any
    tools {
        maven 'Maven' // This should match the name of your Maven installation in Jenkins
        jdk 'JDK12'  // This should match the name of your JDK installation in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git 'https://github.com/GreeshmaSetty/ErrorProne.git'
            }
        }
        stage('Build') {
            steps {
                // Build the project using Maven
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                // Run tests using Maven
                sh 'mvn test -DsuiteXmlFile=src/test/resources/testng.xml'
            }
        }
        stage('Package') {
            steps {
                // Package the application using Maven
                sh 'mvn package'
            }
        }
    }
    post {
        always {
           archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
            junit 'target/surefire-reports/*.xml'
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'target/site', // Directory where your HTML report is located
                reportFiles: 'index.html', // Main HTML file to display
                reportName: 'HTML Report'
            ])
        }
        success {
            // Actions to take on success
            echo 'Build succeeded!'
        }
        failure {
            // Actions to take on failure
            echo 'Build failed!'
        }
    }
}
