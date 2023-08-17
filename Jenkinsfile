pipeline {
    agent any

    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps'
        TARGET_SERVER = '3.234.86.185'
        TARGET_PORT = '8090'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Copy HTML to Tomcat on Dev Server') {
            steps {
                script {
                    def sourceHtmlPath

                    if (env.BRANCH_NAME == 'Dev') {
                        sourceHtmlPath = 'index_dev.html'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    def context = env.BRANCH_NAME.toLowerCase()
                    sshagent(['my-ssh-credentials']) {
                        sh "ssh -p ${TARGET_PORT} user@${TARGET_SERVER} 'mkdir -p ${TOMCAT_WEBAPPS}/${context}'"
                        sh "scp -P ${TARGET_PORT} ${sourceHtmlPath} user@${TARGET_SERVER}:${TOMCAT_WEBAPPS}/${context}/index.html"
                    }
                }
            }
        }
    }
}
