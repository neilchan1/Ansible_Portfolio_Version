pipeline {
    agent any
    options { timestamps() }
    parameters {
        string(name: 'SERVICE_NAME', description: 'Service to start/stop')
        string(name: 'DOWNLOAD_URL', description: 'File to download')
        string(name: 'DOWNLOAD_DEST', description: 'Destination path for download')
        string(name: 'ARCHIVE_PATH', description: 'Archive path to extract')
        string(name: 'EXTRACT_DEST', description: 'Destination path for extraction')
        text(name: 'USERS', description: 'JSON array of users')
    }

    environment {
        SERVICE_NAME = "${params.SERVICE_NAME}"
        DOWNLOAD_URL = "${params.DOWNLOAD_URL}"
        DOWNLOAD_DEST = "${params.DOWNLOAD_DEST}"
        ARCHIVE_PATH = "${params.ARCHIVE_PATH}"
        EXTRACT_DEST = "${params.EXTRACT_DEST}"
        USERS = "${params.USERS}"
    }

    stages {
        stage('Stop Service') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbooks/stop_service.yml',
                    inventory: 'inventory/dev.yml',
                    colorized: true
                )
            }
        }

        stage('Download File') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbooks/download_file.yml',
                    inventory: 'inventory/dev.yml',
                    colorized: true
                )
            }
        }

        stage('Extract File') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbooks/extract_file.yml',
                    inventory: 'inventory/dev.yml',
                    colorized: true
                )
            }
        }

        stage('Manage Users') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbooks/manage_users.yml',
                    inventory: 'inventory/dev.yml',
                    colorized: true
                )
            }
        }

        stage('Start Service') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbooks/start_service.yml',
                    inventory: 'inventory/dev.yml',
                    colorized: true
                )
            }
        }

        stage('Full Site') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbooks/site.yml',
                    inventory: 'inventory/dev.yml',
                    colorized: true
                )
            }
        }
    }
}
