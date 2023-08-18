pipeline {
    agent any
 
    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps'
        DEV_TOMCAT_IP = '54.91.56.196'
        DEV_TOMCAT_PORT = '8090'
        PROD_TOMCAT_IP = '54.197.144.146'
        PROD_TOMCAT_PORT = '8091'
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
                        tomcatWebappsDir = "/manager/text/deploy?path=/&update=true"
                        tomcatUrl = "http://${PROD_TOMCAT_IP}:${PROD_TOMCAT_PORT}${tomcatWebappsDir}"
                    } else if (env.BRANCH_NAME.toLowerCase() == 'dev') {
                        sourceHtmlPath = 'index_dev.html'
                        tomcatWebappsDir = "/manager/text/deploy?path=/&update=true"
                        tomcatUrl = "http://${DEV_TOMCAT_IP}:${DEV_TOMCAT_PORT}${tomcatWebappsDir}"
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    tomcatAuth = "${TOMCAT_USERNAME}:${TOMCAT_PASSWORD}"
                    def context = env.BRANCH_NAME.toLowerCase()

                    sh "curl -T ${sourceHtmlPath} ${tomcatUrl}${context}/index.html --user ${tomcatAuth} --write-out %{http_code} --silent --output /dev/null"
                }
            }
        }
    }
}
