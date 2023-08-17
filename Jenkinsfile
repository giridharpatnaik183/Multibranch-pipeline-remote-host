pipeline {
    agent any

    environment {
        DEV_TOMCAT_IP = '54.224.2.7'
        DEV_TOMCAT_PORT = '8091'
        PROD_TOMCAT_IP = '3.91.72.144'
        PROD_TOMCAT_PORT = '8092'
        TOMCAT_USERNAME = 'tomcat'
        TOMCAT_PASSWORD = 'password'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout scm
            }
        }

        stage('Copy HTML to Tomcat') {
            steps {
                script {
                    def sourceHtmlPath
                    def tomcatWebappsDir
                    def tomcatUrl
                    def tomcatAuth

                    if (env.BRANCH_NAME.toLowerCase() == 'prod') {
                        sourceHtmlPath = 'index_prod.html'
                        tomcatWebappsDir = "/prod"
                        tomcatUrl = "http://${PROD_TOMCAT_IP}:${PROD_TOMCAT_PORT}/manager/text/deploy?path=${tomcatWebappsDir}&update=true"
                    } else if (env.BRANCH_NAME.toLowerCase() == 'dev') {
                        sourceHtmlPath = 'index_dev.html'
                        tomcatWebappsDir = "/dev"
                        tomcatUrl = "http://${DEV_TOMCAT_IP}:${DEV_TOMCAT_PORT}/manager/text/deploy?path=${tomcatWebappsDir}&update=true"
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    tomcatAuth = "${TOMCAT_USERNAME}:${TOMCAT_PASSWORD}"

                    sh "curl -T ${sourceHtmlPath} ${tomcatUrl} --user ${tomcatAuth} --write-out %{http_code} --silent --output /dev/null"
                }
            }
        }
    }
}
