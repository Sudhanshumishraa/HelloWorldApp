pipeline {
    agent any

    environment {
        DOTNET_ROOT = "C:\\Program Files\\dotnet"
        PATH = "${env.PATH};${DOTNET_ROOT};C:\\path\\to\\putty"  // Add pscp.exe directory
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: '8ad3972c-76d4-4a6f-aa27-af093a06ddbc', url: 'https://github.com/Sudhanshumishraa/HelloWorldApp.git', branch: 'main'
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
                    IF EXIST "publish" (
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
                    def remoteUser = 'CylSrv9Mgr'
                    def remotePass = 'Dwunull@515W'
                    def remoteHost = '103.38.50.157'
                    def remotePath = 'D:/CI_CD/test_Dotnet_2/'
                    bat """pscp -pw "${remotePass}" -r publish/* ${remoteUser}@${remoteHost}:${remotePath}"""
                }
            }
        }
    }

    post {
        failure {
            echo '❌ Deployment Failed!'
        }
        success {
            echo '✅ Deployment Succeeded!'
        }
    }
}
