pipeline {
    agent any

    environment {
        NODE_VERSION = '20'
        BUILD_PATH = "${WORKSPACE}\\build"
        DEPLOY_SERVER = "101.38.10.157:58373"
        DEPLOY_USER = "CygglScccrvcsc9Mgr"
        DEPLOY_PASS = "Dwudss$CakLy@515W"
        DEPLOY_PATH = "D:\\CI_CD\\test_ReactJS"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: '3bb88872-897d-441a-afc6-b43fe3d190cb', url: 'http://github.cylsys.com/Opsadmin/DevTest.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'  // Linux command for npm install
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'  // Linux command for npm run build
            }
        }

        stage('Deploy to Windows Server') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Uat-Server', usernameVariable: 'DEPLOY_USER', passwordVariable: 'DEPLOY_PASS')]) {
                        sh """
                        # Use SCP to copy the build files to the Windows server
                        # Make sure OpenSSH Server is installed and running on the Windows server
                        scp -r ${BUILD_PATH}/* ${DEPLOY_USER}@${DEPLOY_SERVER}:${DEPLOY_PATH}
                        """
                    }
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
