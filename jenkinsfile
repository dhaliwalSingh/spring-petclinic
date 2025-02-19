pipeline {
    agent any
    
    // Trigger every 10 minutes on Mondays
    triggers {
        cron('H/10 * * * 1') 
        // Explanation: 'H/10 * * * 1' means:
        // - H/10: hashed start time, repeating every 10 minutes
        // - *: every hour
        // - *: every day of the month
        // - *: every month
        // - 1: Monday (0 is Sunday, 1 is Monday, etc.)
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Clones the repository with the Jenkinsfile
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Clean and compile the project
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                // Run tests, automatically generates Jacoco exec files
                sh 'mvn test'
            }
        }
        
        stage('Jacoco Report') {
            steps {
                // Generate the Jacoco aggregate report
                sh 'mvn jacoco:report'
            }
            post {
                // The Jenkins Jacoco plugin can pick up these reports
                success {
                    echo "Jacoco report generated."
                }
            }
        }
        
        stage('Package') {
            steps {
                // Package the application as a JAR (artifact)
                sh 'mvn package'
            }
        }
    }
    
    post {
        always {
            // Archive the results as needed
            junit '**/target/surefire-reports/*.xml'
            // Archive the JAR artifact, for example:
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}