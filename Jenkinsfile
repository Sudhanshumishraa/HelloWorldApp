pipeline {
    agent any

    environment {
        NODE_VERSION = '20'
        BUILD_PATH = "${WORKSPACE}\\publish"
        DEPLOY_SERVER = "103.38.50.157"
        DEPLOY_USER = "CylSrv9Mgr"
        DEPLOY_PASS = "Dwu\$CakLy@515W"  // No escape needed in double quotes
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

        stage('Verify Build Output') {  
            steps {
                script {
                    def buildExists = fileExists("${BUILD_PATH}")
                    if (!buildExists) {
                        error "❌ Build directory does not exist! Aborting deployment."
                    }
                }
            }
        }

        stage('Deploy to Windows Server') {
            steps {
                bat """
                    "${PSCP_PATH}" -batch -r -pw -p 58373 "${DEPLOY_PASS}" "${BUILD_PATH}\\*" ${DEPLOY_USER}@${DEPLOY_SERVER}:${DEPLOY_PATH}
                """
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
