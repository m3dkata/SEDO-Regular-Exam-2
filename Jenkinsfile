pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:8.0'
            args '-u root:root'
        }
    }
    
    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = 'true'
        DOTNET_SKIP_FIRST_TIME_EXPERIENCE = 'true'
    }
    
    triggers {
        githubPush()
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        
        stage('Restore Dependencies') {
            steps {
                echo 'Restoring .NET dependencies...'
                sh 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'dotnet build --no-restore --configuration Release'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'dotnet test --no-build --configuration Release --verbosity normal'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded! All tests passed.'
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
    }
}
