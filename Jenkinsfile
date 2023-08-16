pipeline {
    agent any

    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps' // Set this to the actual path of the Tomcat webapps directory
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
                    def tomcatWebappsDir = "/var/lib/tomcat9/webapps/ROOT/"
                    def sourceHtmlPath

                    // Determine the source index.html based on the branch
                    if (env.BRANCH_NAME == 'prod') {
                        sourceHtmlPath = 'index_prod.html'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sourceHtmlPath = 'index_dev.html'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    // Copy the appropriate index.html to Tomcat
                    sh "cp ${sourceHtmlPath} ${tomcatWebappsDir}/index.html"
                }
            }
        }
    }
}
