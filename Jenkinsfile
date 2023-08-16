pipeline {
    agent any

    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps' // Set this to the actual path of the Tomcat webapps directory
        TARGET_SERVER = '3.234.86.185'  // IP address of the target server
        TARGET_PORT = '8090'            // Tomcat port on the target server
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout scm
            }
        }

        stage('Copy HTML to Tomcat on Dev Server') {
            steps {
                script {
                    def sourceHtmlPath

                    // Determine the source index.html based on the branch
                    if (env.BRANCH_NAME == 'Dev') {
                        sourceHtmlPath = 'index_dev.html'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    // Copy the appropriate index.html to the target Tomcat server
                    def context = env.BRANCH_NAME.toLowerCase() // Use lowercase branch name as context
                    sh "ssh user@${TARGET_SERVER} -p ${TARGET_PORT} 'mkdir -p ${TOMCAT_WEBAPPS}/${context}'"
                    sh "scp -P ${TARGET_PORT} ${sourceHtmlPath} user@${TARGET_SERVER}:${TOMCAT_WEBAPPS}/${context}/index.html"
                }
            }
        }
    }
}
