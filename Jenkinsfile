pipeline {
    agent any
 
    environment {
        NODE_VERSION = '20'
        BUILD_PATH = "${WORKSPACE}/publish"
        DEPLOY_SERVER = "103.38.50.157"
        DEPLOY_USER = "CylSrv9Mgr"
        DEPLOY_PASS = "Dwu\$CakLy@515W"
        DEPLOY_PATH = "D:/CI_CD/test_Dotnet_2/"  // ✅ Fixed Windows path
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
        bat 'dotnet publish HelloWorld.csproj -c Release -o /var/jenkins_home/jobs/Dotnet/workspace/publish'
    	}
	}
 
 
        stage('Verify Build Output') { // ✅ Check if build directory exists
            steps {
                script {
                    def buildExists = bat(script: "test -d ${BUILD_PATH} && echo 'exists'", returnStdout: true).trim()
                    if (buildExists != "exists") {
                        error "❌ Build directory does not exist! Aborting deployment."
                    }
                }
            }
        }
 
        stage('Install SSHPass (if missing)') {
            steps {
                script {
                    def sshpassExists = sh(script: "command -v sshpass", returnStatus: true) == 0
                    if (!sshpassExists) {
                        bat 'sudo apt update && sudo apt install -y sshpass'
                    }
                }
            }
        }
 
        stage('Deploy to Windows Server') {
            steps {
                script {
                    bat '''
                        export SSHPASS="$DEPLOY_PASS"
                        sshpass -e scp -o StrictHostKeyChecking=no -r "$BUILD_PATH/." "$DEPLOY_USER@$DEPLOY_SERVER:$DEPLOY_PATH"
                    '''
                }
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
 
