pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend (.NET)') {
            steps {
                bat 'cd frontend\\EasyDevOps && dotnet restore'
                bat 'cd frontend\\EasyDevOps && dotnet build -c Release'
            }
        }

        stage('Security Test (Dependency-Check)') {
            steps {
                dependencyCheck additionalArguments: '--scan "frontend\\EasyDevOps" --format HTML', odcInstallation: 'Default'
                dependencyCheckPublisher pattern: '**/dependency-check-report.html'
            }
        }
    }
}
