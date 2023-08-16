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
                    def tomcatWebappsDir = "/var/lib/tomcat9/webapps"
                    def sourceHtmlPath

                    // Determine the source index.html based on the branch
                    if (env.BRANCH_NAME == 'Prod') {
                        sourceHtmlPath = 'index_prod.html'
                    } else if (env.BRANCH_NAME == 'Dev') {
                        sourceHtmlPath = 'index_dev.html'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    // Copy the appropriate index.html to Tomcat in a separate context
                    def context = env.BRANCH_NAME.toLowerCase() // Use lowercase branch name as context
                    sh "mkdir -p ${tomcatWebappsDir}/${context}"
                    sh "cp ${sourceHtmlPath} ${tomcatWebappsDir}/${context}/index.html"
                }
            }
        }
    }
}
