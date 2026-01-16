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
                // Scan de hele repo, maak HTML report, geen online updates (voorkomt NVD errors),
                // disableAssembly voorkomt extra .NET-binary gedoe op Windows.
                dependencyCheck additionalArguments: '''
                    --scan "."
                    --format HTML
                    --out "dependency-check-report"
                    --noupdate
                    --disableAssembly
                '''.trim(), odcInstallation: 'Default'

                dependencyCheckPublisher pattern: '**/dependency-check-report/dependency-check-report.html'
            }
        }
    }
}
