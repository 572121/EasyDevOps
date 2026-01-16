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
                // Draai de scan, maar laat de build niet falen als er niets te scannen is.
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    dependencyCheck additionalArguments: '''
                        --scan "."
                        --format HTML
                        --out "dependency-check-report"
                        --noupdate
                        --disableAssembly
                    '''.trim(), odcInstallation: 'Default'
                }

                // Probeer report te publiceren als die er is
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    dependencyCheckPublisher pattern: '**/dependency-check-report/dependency-check-report.html'
                }
            }
        }
    }
}
