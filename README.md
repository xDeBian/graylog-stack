# Graylog Ansible Role

This Ansible role automates the deployment and configuration of Graylog with support for both Elasticsearch and OpenSearch backends, along with MongoDB for data storage.

## Features

- Complete deployment of Graylog logging infrastructure
- Support for both Elasticsearch and OpenSearch as backend options
- MongoDB configuration with secure replication options
- Kibana and OpenSearch Dashboards for visualization
- SSL/TLS support for secure communications
- Backup and restore functionality
- Version management and upgrade capabilities
- Metrics collection via Metricbeat

## Requirements

- Ansible 2.9+
- Ubuntu 22.04 (Jammy) target hosts
- Root or sudo access on target hosts

## Essential Variables

### Graylog Configuration
- `graylog_repo_version` - Repository version for Graylog
- `graylog_version` - Graylog version to install
- `graylog_listen_port` - Port for Graylog web interface
- `graylog_root_username` - Admin username
- `graylog_password_secret` - Secret key for password encryption **(IMPORTANT: CHANGE THIS)**
- `graylog_root_password_sha2` - SHA2 hash of the root password **(IMPORTANT: CHANGE THIS)**
- `graylog_root_timezone` - Default timezone setting

### MongoDB Configuration
- `mongodb_version` - MongoDB version to install
- `mongodb_user` - MongoDB admin username
- `mongodb_password` - MongoDB admin password **(IMPORTANT: CHANGE THIS)**
- `mongodb_repl_set_name` - Replication set name
- `mongodb_graylog_user` - Dedicated Graylog user
- `mongodb_graylog_password` - Password for Graylog user **(IMPORTANT: CHANGE THIS)**

### Elasticsearch Configuration
- `elasticsearch_version` - Elasticsearch version to install
- `elasticsearch_cluster` - Cluster name
- `elasticsearch_http_port` - HTTP port
- `jvm_Xms_size` - Initial heap size
- `jvm_Xmx_size` - Maximum heap size
- `elasticsearch_password` - Admin password **(IMPORTANT: CHANGE THIS)**

### OpenSearch Configuration
- `opensearch_version` - OpenSearch version to install
- `opensearch_cluster` - Cluster name
- `opensearch_http_port` - HTTP port
- `opensearch_password` - Admin password **(IMPORTANT: CHANGE THIS)**

### Backup Configuration
- `aws_access_key` - AWS access key for S3 backups **(IMPORTANT: CHANGE THIS)**
- `aws_secret_access_key` - AWS secret key **(IMPORTANT: CHANGE THIS)**
- `s3_bucket` - S3 bucket name for backups

## Task Modules

### Main Tasks
- **main.yml**: Entry point that includes appropriate task files based on variables

### Elasticsearch Tasks
- **elasticsearch_install.yml**: Installs and configures Elasticsearch
- **elasticsearch_configuration.yml**: Manages configuration files and security settings
- **elasticsearch_metricbeat.yml**: Sets up Metricbeat for monitoring
- **elasticsearch_uninstaller.yml**: Removes Elasticsearch
- **es_upgrade.yml**: Handles version upgrades

### Graylog Tasks
- **graylog_install.yml**: Installs Graylog with selected backend
- **graylog_configuration.yml**: Configures Graylog server
- **graylog_uninstall.yml**: Removes Graylog
- **upgrade_graylog.yml**: Handles version upgrades/downgrades

### MongoDB Tasks
- **mongodb_install.yml**: Installs MongoDB server
- **mongodb_configuration.yml**: Configures MongoDB settings
- **mongodb_create_admin_user.yml**: Creates admin user
- **mongodb_create_graylog_user.yml**: Creates dedicated Graylog user
- **mongodb_replication.yml**: Sets up replication for high availability
- **mongodb_uninstall.yml**: Removes MongoDB

### Kibana Tasks
- **kibana_install.yml**: Installs Kibana
- **kibana_configuration.yml**: Configures Kibana settings
- **kibana_create_cert.yml**: Generates SSL certificates
- **kibana_uninstaller.yml**: Removes Kibana

### OpenSearch Tasks
- **opensearch_install.yml**: Installs OpenSearch
- **opensearch_configuration.yml**: Configures OpenSearch
- **opensearch_uninstaller.yml**: Removes OpenSearch

### OpenSearch Dashboards Tasks
- **dashboards_install.yml**: Installs OpenSearch Dashboards
- **dashboards_uninstall.yml**: Removes OpenSearch Dashboards

### Backup and Restore Tasks
- **dump.yml**: Creates MongoDB backups
- **restore.yml**: Restores MongoDB from backups

## Usage

### Basic Usage

1. Add your servers to the inventory file
2. Configure essential variables
3. Run the playbook:

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml
```

### Example Playbook

```yaml
---
- name: Install Opensearch
  hosts: opensearch    
  serial: 1
  become: yes
  vars_files:
    - default/main.yml
  tasks:
    
    - name: Include OpenSearch install  tasks
      include_tasks: tasks/Opensearch/opensearch_install.yml

    - name: Include OpenSearch configuration tasks
      include_tasks: tasks/Opensearch/opensearch_configuration.yml

```

## Security

**Important:** Update all default passwords before deploying to production!

## License

MIT