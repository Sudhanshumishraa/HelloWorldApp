pipeline {
    agent any

    environment {
        BUILD_PATH = "${WORKSPACE}\\publish"
        DEPLOY_SERVER = "103.38.50.157"
        DEPLOY_USER = "CylSrv9Mgr"
        DEPLOY_PASS = "Dwu$CakLy@515W"
        DEPLOY_PATH = "D:/CI_CD/test_Dotnet_2/"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: '8ad3972c-76d4-4a6f-aa27-af093a06ddbc', url: 'https://github.com/Sudhanshumishraa/HelloWorldApp.git'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build HelloWorldApp.csproj --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                bat 'dotnet publish HelloWorldApp.csproj -c Release -o publish'
            }
        }

        stage('Verify Build Output') {
            steps {
                bat '''
                    IF EXIST "%WORKSPACE%\\publish" (
                        echo ✅ Publish directory exists.
                    ) ELSE (
                        echo ❌ Publish directory does not exist.
                        exit /b 1
                    )
                '''
            }
        }

        stage('Check sshpass in PATH') {
            steps {
                bat '''
                    where sshpass >nul 2>nul
                    IF ERRORLEVEL 1 (
                        echo ❌ ERROR: sshpass.exe not found in PATH.
                        exit /b 1
                    ) ELSE (
                        echo ✅ sshpass.exe found in PATH.
                    )
                '''
            }
        }

        stage('Deploy to Windows Server') {
            steps {
                bat '''
                    echo Setting SSHPASS environment variable...
                    set SSHPASS=%DEPLOY_PASS%
                    echo ✅ SSHPASS set.

                    echo Starting deployment via sshpass and scp...
                    sshpass -e scp -o StrictHostKeyChecking=no -r "%BUILD_PATH%\\*" %DEPLOY_USER%@%DEPLOY_SERVER%:%DEPLOY_PATH%
                    IF ERRORLEVEL 1 (
                        echo ❌ Deployment failed during SCP.
                        exit /b 1
                    ) ELSE (
                        echo ✅ Deployment completed successfully.
                    )
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
