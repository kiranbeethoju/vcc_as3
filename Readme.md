# GCP Autoscaling Deployment Script

## Overview

This Bash script automates the deployment of a Flask API application to Google Cloud Platform (GCP) with automatic scaling capabilities. The script monitors local CPU usage and creates or destroys GCP VM instances based on configurable thresholds.

## Features

- **Automated GCP Authentication**: Handles service account authentication
- **CPU Usage Monitoring**: Dynamically creates or removes VMs based on local CPU usage
- **Managed Instance Groups**: Creates instance templates and managed instance groups
- **Autoscaling Configuration**: Sets up CPU-based autoscaling policies
- **Data Transfer**: Transfers local data to GCP VMs
- **Application Deployment**: Automatically deploys a simple Flask API
- **Nginx Configuration**: Sets up Nginx as a reverse proxy
- **Load Testing**: Includes tools for testing the autoscaling capabilities

## Prerequisites

- Google Cloud SDK (gcloud) installed and configured
- Service account JSON key file
- Active GCP project
- Linux/Unix environment with bash support
- Required utilities: `bc`, `top`, `date`

## Configuration Variables

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `GCP_ZONE` | GCP zone to deploy in | asia-south2-b |
| `LOCAL_DATA_PATH` | Local data path to upload | $HOME/local/data |
| `REMOTE_DATA_PATH` | Remote data path on VM | $HOME |
| `GCP_USER` | SSH user for VM access | vboxuser |
| `CPU_THRESHOLD` | CPU threshold for scaling | 75% |
| `APP_DIR` | App directory name on VM | myapp |
| `PORT` | Application port | 8080 |

## Installation

1. Clone this repository or download the script
2. Make the script executable:
   ```bash
   chmod +x gcp_autoscaling_deploy.sh
   ```
3. Ensure your GCP service account key file (as3-g24ai1115-e07b4f7cb21b.json) is in the same directory

## Usage

Run the script with:

```bash
./gcp_autoscaling_deploy.sh
```

## How It Works

1. **Authentication**: The script authenticates with GCP using the service account key.
2. **CPU Monitoring**: Checks the current CPU usage of the local system.
3. **VM Management**:
   - If CPU usage exceeds the threshold and no VM exists, it creates a new VM.
   - If CPU usage drops below the threshold and a VM exists, it deletes the VM.
4. **Application Deployment**: When creating a VM, it:
   - Installs dependencies (Python, pip, nginx)
   - Sets up a Python virtual environment
   - Deploys a simple Flask application
   - Configures systemd service for running the app
   - Sets up Nginx as a reverse proxy
   - Performs a load test to verify autoscaling

## Autoscaling Policy

The script configures the following autoscaling parameters:
- Maximum instances: 3
- Minimum instances: 1
- Target CPU utilization: 75%
- Cool down period: 20 seconds

## Logs

The script provides colored log output with timestamps for easy monitoring:
-  INFO: General information
-  WARNING: Warning messages
-  ERROR: Error messages
-  COMMAND: Command execution
-  HEADER: Section headers
-  SUCCESS: Success messages

## Troubleshooting

- **Authentication Failures**: Ensure the service account key file is correct and has proper permissions.
- **SSH Connection Issues**: Check that the GCP_USER is valid and has SSH access.
- **Data Transfer Problems**: Verify the paths (LOCAL_DATA_PATH and REMOTE_DATA_PATH) are valid.
- **VM Creation Errors**: Check GCP quotas and permissions for your project.

## Security Considerations

- The script contains a service account key filename. Ensure this file is properly secured.
- Consider storing sensitive configuration in environment variables rather than hardcoding.
- Review firewall rules on GCP to ensure only necessary ports are exposed.

## Customization

To customize the application:
1. Modify the Flask app code in the script to deploy your own application.
2. Adjust the VM instance type (currently e2-medium) as needed for your workload.
3. Change the autoscaling parameters to match your application's requirements.


