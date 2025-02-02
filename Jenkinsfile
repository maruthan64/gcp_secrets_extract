// Define variables globally
def tower_encrypted_user
def tower_encrypted_pwd

node {
    // Get configurations based on branch and config file name
    def comm_conf = "secrets/secrets.yml"
    def commonyml = readYaml(file: comm_conf)
    tower_encrypted_user = commonyml.tower.tower_encrypted_user
    tower_encrypted_pwd = commonyml.tower.tower_encrypted_pwd

    echo "user: $tower_encrypted_user"
    echo "pwd: $tower_encrypted_pwd"
}

// Now you can use tower_encrypted_user and tower_encrypted_pwd globally
echo "Global user: $tower_encrypted_user"
echo "Global pwd: $tower_encrypted_pwd"

// Example usage in a YAML context
def yamlContent = """
"environmentType": "Dev",
"environmentName": "Development",
"tower_host": "${project_config.tower.tower_host}",
"tower_encrypted_user": "${tower_encrypted_user}",
"tower_encrypted_pwd": "${tower_encrypted_pwd}",
"job_template": "${job_template}",
"ansible_Extras": "${ansible_extra}",
"playbook": "gcp_sndbx_cli_host_cd.yml"
"""

// Print the YAML content
echo yamlContent