pipeline {
    agent any

    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps' // Set this to the actual path of the Tomcat webapps directory
        REMOTE_SERVER_IP = '54.226.135.62'
        TOMCAT_PORT = '8090'
        REMOTE_TOMCAT_WEBAPPS = "/var/lib/tomcat9/webapps" // Path on the remote server
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout scm
            }
        }

        stage('Deploy HTML to Remote Tomcat') {
            steps {
                script {
                    def sourceHtmlPath

                    // Determine the source index.html based on the branch
                    if (env.BRANCH_NAME == 'Prod') {
                        sourceHtmlPath = 'index_prod.html'
                    } else if (env.BRANCH_NAME == 'Dev') {
                        sourceHtmlPath = 'index_dev.html'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    // Copy the HTML file to the remote server using SSH
                    def context = env.BRANCH_NAME.toLowerCase() // Use lowercase branch name as context
                    sshPublisher(publishers: [sshPublisherDesc(
                        configName: 'my_ssh_credentials', // Name of the configured SSH credentials in Jenkins
                        transfers: [
                            sshTransfer(
                                sourceFiles: "${sourceHtmlPath}",
                                removePrefix: "",
                                remoteDirectory: "${REMOTE_TOMCAT_WEBAPPS}/${context}"
                            )
                        ]
                    )])
                    
                    // Restart Tomcat on the remote server
                    sshPublisher(publishers: [sshPublisherDesc(
                        configName: 'my_ssh_credentials', // Name of the configured SSH credentials in Jenkins
                        transfers: [
                            sshTransfer(
                                execCommand: "sudo service tomcat9 restart"
                            )
                        ]
                    )])
                }
            }
        }
    }
}
