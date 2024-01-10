pipeline {
    agent any

    environment {
        FLASK_APP_URL = 'http://darioserver.duckdns.org:5001'
    }

    stages {
        stage('Logical Coupling') {
            steps {
                script {
                    catchError(buildResult: 'FAILURE', stageResult: 'FAILURE'){                        
                        executeCouplingStage('logical-coupling')
                    }   
                }
            }
      
        }

        stage('Developer Coupling') {
            steps {
                script {
                    catchError(buildResult: 'FAILURE', stageResult: 'FAILURE'){
                        executeCouplingStage('developer-coupling')

                    }

                }
            }
        }
    }

}

def executeCouplingStage(couplingType) {
    def repoUrl = env.GIT_URL
    def commitHash = env.GIT_COMMIT
    def branchName = 'main'  // You can customize this if needed

    echo "Repository URL: ${repoUrl}"
    echo "Commit Hash: ${commitHash}"
    echo "Branch Name: ${branchName}"

    def response = sh(script: """
        curl -G -d 'git_url=${repoUrl}' -d 'commit_hash=${commitHash}' -d 'branch=${branchName}' ${FLASK_APP_URL}/${couplingType}
    """, returnStdout: true).trim()

    echo "Raw Response from Flask App: ${response}"

    def jsonResponse = readJSON text: response
    def exitCode = jsonResponse.exit_code
    def message = jsonResponse.message

    echo "Exit Code: ${exitCode}"
    echo "Message: ${message}"

    if (exitCode != 0) {
        error "Stage failed. Exit Code: ${exitCode}, Message: ${message}"
    }
}
