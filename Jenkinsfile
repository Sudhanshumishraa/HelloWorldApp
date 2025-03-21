pipeline {
    agent any
 
    environment {
        BUILD_PATH = "${WORKSPACE}\\publish"
        DEPLOY_SERVER = "103.38.50.157"
        DEPLOY_USER = "CylSrv9Mgr"
        DEPLOY_PASS = "Dwu\$CakLy@515W"
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
                        echo Directory exists
                    ) ELSE (
                        echo Directory does not exist
                        exit /b 1
                    )
                '''
            }
        }
        
 stage('Deploy to Windows Server') {
    steps {
        script {
            bat '''
                set SSHPASS=%DEPLOY_PASS%
                sshpass -e scp -o StrictHostKeyChecking=no -r "%BUILD_PATH%\\." "%DEPLOY_USER%@%DEPLOY_SERVER%:%DEPLOY_PATH%"
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
