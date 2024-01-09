pipeline {
    agent any

    stages {
        stage('Trigger Flask App') {
            steps {
                script {
                    
                    echo "Branch Name of the Commit Performed: ${branchName}"
                    echo "Commit Hash of the Commit Performed: ${commitHash}"
                    echo "Branch Name in Which the Build is Committing: ${committingBranch}"


                    // Replace the URL with the actual URL of your Flask app
                    def flaskAppUrl = 'http://darioserver.duckdns.org:5001/logical-coupling'
                    
                    // Example parameters to pass to the Flask app
                            // Get the repository URL
                    def repoUrl = env.GIT_URL

                    // Get the commit hash
                    def commitHash = env.GIT_COMMIT

                    // Get the branch name
                    def branchName = env.BRANCH_NAME
                    
                    // Use curl to make an HTTP request to the Flask app
                    def response = sh(script: "curl -X GET ${flaskAppUrl}?git_url=${repoUrl}&commit_hash=${commitHash}&branch=${branchName}", returnStdout: true).trim()


                    echo "Raw Response from Flask App: ${response}"

                    // Parse the JSON response
                    def jsonResponse = readJSON text: response

                    // Access exit code and message from the JSON response
                    def exitCode = jsonResponse.exit_code
                    def message = jsonResponse.message

                    echo "Exit Code: ${exitCode}"
                    echo "Message: ${message}"

                    // Determine whether the stage should pass or fail
                    if (exitCode == 0) {
                        echo "Stage passed."
                    } else {
                        error "Stage failed. Exit Code: ${exitCode}, Message: ${message}"
                    }
                }
            }
        }
    }

}
