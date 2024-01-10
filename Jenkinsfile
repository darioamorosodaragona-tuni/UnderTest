pipeline {
    agent any

    environment {
        FLASK_APP_URL = 'http://darioserver.duckdns.org:5001'
    }

    stages {
        stage('Logical Coupling') {
            steps {
                script {
                    try {
                        
                        executeCouplingStage('logical-coupling')

                    }
                    
                    catchError(buildResult: 'FAILURE', stageResult: 'SUCCESS'){}
                }
            }
      
        }

        stage('Developer Coupling') {
            steps {
                script {
                    try {
                    
                        executeCouplingStage('developer-coupling')
                    
                    }
                    
                    catchError(buildResult: 'FAILURE', stageResult: 'SUCCESS'){}

                }
            }
        }
    }

    post {
        always {
            echo "Post-build actions after all stages"
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

    if (exitCode == 0) {
        echo "Stage passed."
    } else {
        error "Stage failed. Exit Code: ${exitCode}, Message: ${message}"
    }
}
