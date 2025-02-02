// Define variables globally
def tower_host
def tower_encrypted_user
def tower_encrypted_pwd

node {
    // Get configurations based on branch and config file name
    def comm_conf = "secrets/secrets.yml"
    def commonyml = readYaml(file: comm_conf)
    
    // Assuming you have an environment variable or parameter to determine the environment
    def envName = "dev" // Change this to your actual environment variable or parameter

    tower_host = commonyml.secrets[envName].tower_host
    tower_encrypted_user = commonyml.secrets[envName].tower_encrypted_user
    tower_encrypted_pwd = commonyml.secrets[envName].tower_encrypted_pwd

    echo "user: $tower_encrypted_user"
    echo "pwd: $tower_encrypted_pwd"
}

// Now you can use tower_host, tower_encrypted_user, and tower_encrypted_pwd globally
echo "Global host: $tower_host"
echo "Global user: $tower_encrypted_user"
echo "Global pwd: $tower_encrypted_pwd"

// Example usage in a YAML context
def yamlContent = """
"environmentType": "Dev",
"environmentName": "Development",
"tower_host": "${tower_host}",
"tower_encrypted_user": "${tower_encrypted_user}",
"tower_encrypted_pwd": "${tower_encrypted_pwd}",
"job_template": "${job_template}",
"ansible_Extras": "${ansible_extra}",
"playbook": "gcp_sndbx_cli_host_cd.yml"
"""

// Print the YAML content
echo yamlContent