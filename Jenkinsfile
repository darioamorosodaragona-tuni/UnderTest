User
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Step 1: Checkout a project from a Git repo
               sh 'git clone https://github.com/darioamorosodaragona-tuni/LogicalCouplingTool.git'
               //checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/darioamorosodaragona-tuni/LogicalCouplingTool.git']])
            }
        }

        stage('Install Dependencies') {
            steps {
                // Step 2: Install Python packages from requirements.txt
                sh 'cd LogicalCouplingTool'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Script1') {
            steps {
                script {
                    // Step 3: Run script1.py with arguments and capture exit code and output
                    def scriptOutput = sh(script: "python LogicalCouplingNoDb/src/logical_coupling.py --repo_url ${env.GIT_URL} --branch_name ${env.BRANCH_NAME} --commit_hash ${env.GIT_COMMIT}", returnStatus: true, returnStdout: true)

                    // Step 4: Save the exit code and output
                    env.SCRIPT_EXIT_CODE = scriptOutput.exitCode
                    env.SCRIPT_OUTPUT = scriptOutput.stdout.trim()

                    // Step 5: Fail the stage if the exit code is 1
                    if (env.SCRIPT_EXIT_CODE == 1) {
                        error "Script1 failed with exit code 1."
                    }
                }
            }
        }
    }

    post {
        always {
            // Display the script output
            echo "Script Output: ${env.SCRIPT_OUTPUT}"
        }
    }
}
