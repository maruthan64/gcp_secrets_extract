// ✅ Declare df globally (without `def`)
df = [:]

node {
    echo "---> Gather BUILD Configurations <---"
    checkout scm

    project_config = loadProjectConfig()

    // Read YAML
    def envName = "qa"
    def comm_conf = "secrets/secrets.yml"
    def commonyml = readYaml(file: comm_conf)


    df.tower_encrypted_user = commonyml.tower.tower_encrypted_user
    df.tower_encrypted_pwd = commonyml.tower.tower_encrypted_pwd

    echo "DEBUG: User -> ${df.tower_encrypted_user}"
    echo "DEBUG: Password -> ${df.tower_encrypted_pwd}"

}

withEnv(envArr) {
    stage('Building Wheel') {
        echo "---------------------------- BUILD WHEEL --------------------------------"

        // Pipeline environment values
        pipelineEnvironments = """
        [{
          "environmentType": "Dev",
          "environmentName": "Development",
          "tower_host": "${project_config.tower.tower_host}",
          "tower_encrypted_user": "${df.tower_encrypted_user}",
          "tower_encrypted_pwd": "${df.tower_encrypted_pwd}",
          "job_template": "${job_template}",
          "ansible_Extras": "${ansible_extra}"
        }]
        """

        echo "DEBUG: pipelineEnvironments -> ${pipelineEnvironments}"
    }

    // ✅ Deploy Workflow to Spectrum Edge Node (Conditional Deployment)
    if (deploy_workflows && yml.publish.executePublishArtifact && deploy_edgenode) {
        stage('Workflow Edge Node Deployment') {
            script {
                echo "---> Deploying to Edge Node <---"

                def ansible_extra = "-e branch_name=$branch_name -e build_number=$build_number -e deploy_lane=$deploy_env \
                                    -e committer_name=$committer_name -e artifact_type='whl' \
                                    -e notification_email=$committer_email -e buildUrl=$buildUrl"

                deliveryPipeline {
                    deployDev = true
                    pipelineEnvironments = """
                    [{
                        "environmentType": "Dev",
                        "environmentName": "Development",
                        "tower_host": "${project_config.tower.tower_host}",
                        "tower_encrypted_user": "${df.tower_encrypted_user}",
                        "tower_encrypted_pwd": "${df.tower_encrypted_pwd}",
                        "job_template": "${job_template}",
                        "ansible_Extras": "${ansible_extra}"
                    }]
                    """
                }
            }
        }
    }

    // ✅ Deploy Workflow to Spectrum and UAS App Hosts (Conditional Deployment)
    if (deploy_workflows && yml.publish.executePublishArtifact) {
        stage('Workflow Wheel UAS Deployment') {
            script {
                echo "---> Deploying to UAS App Hosts <---"

                def ansible_extra = "-e branch_name=$branch_name -e build_number=$build_number -e deploy_lane=$deploy_env \
                                    -e committer_name=$committer_name -e artifact_type='whl' \
                                    -e notification_email=$committer_email -e buildUrl=$buildUrl"

                deliveryPipeline {
                    deployDev = true
                    pipelineEnvironments = """
                    [{
                        "environmentType": "Dev",
                        "environmentName": "Development",
                        "tower_host": "${project_config.tower.tower_host}",
                        "tower_encrypted_user": "${df.tower_encrypted_user}",
                        "tower_encrypted_pwd": "${df.tower_encrypted_pwd}",
                        "job_template": "${job_template}",
                        "ansible_Extras": "${ansible_extra}"
                    }]
                    """
                }
            }
        }
    }
}
