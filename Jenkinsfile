pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build Frontend (.NET)') {
            steps {
                bat 'cd frontend\\EasyDevOps && dotnet restore'
                bat 'cd frontend\\EasyDevOps && dotnet build -c Release'
            }
        }

        stage('Security Test (Dependency-Check)') {
            steps {
                // Run scan, maar faal de pipeline nooit op exit code
                bat '''
                set DC_DIR=C:\\ProgramData\\Jenkins\\.jenkins\\tools\\org.jenkinsci.plugins.DependencyCheck.tools.DependencyCheckInstallation\\Default
                if exist "%DC_DIR%\\bin\\dependency-check.bat" (
                  "%DC_DIR%\\bin\\dependency-check.bat" --scan "." --format HTML --out "dependency-check-report" --noupdate --disableAssembly
                ) else (
                  "%DC_DIR%\\dependency-check.bat" --scan "." --format HTML --out "dependency-check-report" --noupdate --disableAssembly
                )
                exit /b 0
                '''
            }
        }
    }
}
