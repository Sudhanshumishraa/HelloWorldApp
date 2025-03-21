pipeline {
    agent any

      environment {
        NODE_VERSION = '20'
        BUILD_PATH = "${WORKSPACE}\\publish"
        DEPLOY_SERVER = "103.38.50.157"
        DEPLOY_USER = "CylSrv9Mgr"
        DEPLOY_PASS = "Dwu$CakLy@515W"  // No escape needed in double quotes
        DEPLOY_PATH = "D:/CI_CD/test_Dotnet_2/"
        PSCP_PATH = "C:\\Program Files\\PuTTY\\pscp.exe"
    }

     stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                    credentialsId: '8ad3972c-76d4-4a6f-aa27-af093a06ddbc', 
                    url: 'https://github.com/Sudhanshumishraa/HelloWorldApp.git'
            }
        }

       stage('Build') {
            steps {
                bat 'dotnet build HelloWorldApp.csproj --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                bat "dotnet publish HelloWorldApp.csproj -c Release -o ${BUILD_PATH}"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'CylSrv9Mgr', passwordVariable: 'Dwu$CakLy@515W', usernameVariable: 'CREDENTIAL_USERNAME')]) {
                    powershell '''
                    
                    $credentials = New-Object System.Management.Automation.PSCredential($env:CREDENTIAL_USERNAME, (ConvertTo-SecureString $env:CREDENTIAL_PASSWORD -AsPlainText -Force))

                    
                    New-PSDrive -Name X -PSProvider FileSystem -Root "\\\\LAPTOP-DFRQ3ILG\\coreapp" -Persist -Credential $credentials

                    
                    Copy-Item -Path '.\\publish\\*' -Destination 'X:\' -Force

                    
                    Remove-PSDrive -Name X
                    '''
                }
                }
            }
        }
    }

    post {
        success {
            echo 'Build, test, and publish successful!'
        }
    }
}
