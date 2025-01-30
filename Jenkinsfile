pipeline {
    agent any
    parameters {
        string(name: 'ENV_NAME', defaultValue: 'dev', description: 'Environment name (dev, qa, prod) to load secrets for')
    }
    stages {
        stage('Load Secrets') {
            steps {
                script {
                    // Define the environment from input parameter
                    def envName = params.ENV_NAME
                    def secretsFile = 'secrets.yml'

                    // Validate file existence
                    if (!fileExists(secretsFile)) {
                        error "Secrets file does not exist: ${secretsFile}"
                    }

                    // Read the secrets YAML file
                    def secrets = readYaml(file: secretsFile)

                    // Check if the environment exists in the secrets file
                    if (!secrets.secrets[envName]) {
                        error "Secrets for environment '${envName}' not found in ${secretsFile}"
                    }

                    // Extract environment-specific secrets
                    def envSecrets = secrets.secrets[envName]

                    // Validate necessary secrets
                    if (!envSecrets.tower_host || !envSecrets.tower_encrypted_user || !envSecrets.tower_encrypted_pwd) {
                        error "Secrets file is missing required keys (tower_host, tower_encrypted_user, tower_encrypted_pwd) for '${envName}'."
                    }

                    // Assign secrets to Jenkins environment variables
                    env.TOWER_HOST = envSecrets.tower_host
                    env.TOWER_USER = envSecrets.tower_encrypted_user
                    env.TOWER_PWD = envSecrets.tower_encrypted_pwd

                    echo "Secrets for '${envName}' loaded successfully."
                }
            }
        }

        stage('Use Secrets') {
            steps {
                echo "Using secrets in the pipeline..."
                echo "Tower Host: ${env.TOWER_HOST}"
                echo "Tower User: ${env.TOWER_USER}"
                echo "Tower Password: ${env.TOWER_PWD}"
            }
        }
    }
}