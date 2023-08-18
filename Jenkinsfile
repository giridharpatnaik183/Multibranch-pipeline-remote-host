pipeline {
    agent any
 
    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps'
        REMOTE_USERNAME = 'RemoteServer'
        REMOTE_HOST = '54.226.135.62'
        REMOTE_PORT = '22' // SSH port for the remote instance
        REMOTE_WEBAPPS = '/var/lib/tomcat9/webapps' // Adjust this to the actual path on the remote instance
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Copy HTML to Remote Instance') {
            steps {
                script {
                    def sourceHtmlPath

                    if (env.BRANCH_NAME == 'Prod') {  // Use uppercase 'Prod'
                        sourceHtmlPath = 'index_prod.html'
                    } else if (env.BRANCH_NAME == 'Dev') {  // Use uppercase 'Dev'
                        sourceHtmlPath = 'index_dev.html'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    def remoteDir = "${REMOTE_USERNAME}@${REMOTE_HOST}:${REMOTE_PORT}:${REMOTE_WEBAPPS}/${env.BRANCH_NAME.toLowerCase()}"
                    sh "sshpass -p my_ssh_credentials ssh -p ${REMOTE_PORT} ${remoteDir} 'mkdir -p ${REMOTE_WEBAPPS}/${env.BRANCH_NAME.toLowerCase()}'"
                    sh "sshpass -p my_ssh_credentials scp -P ${REMOTE_PORT} ${sourceHtmlPath} ${remoteDir}/index.html"
                }
            }
        }

        stage('Remote Tomcat Restart') {
            steps {
                script {
                    def remoteDir = "${REMOTE_USERNAME}@${REMOTE_HOST} -p ${REMOTE_PORT}"
                    sh "sshpass -p my_ssh_credentials ssh -p ${REMOTE_PORT} ${remoteDir} 'sudo service tomcat9 restart'"
                }
            }
        }
    }
}
