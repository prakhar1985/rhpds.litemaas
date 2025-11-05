# LiteMaaS Ansible Collection for RHPDS

Cloud-agnostic Ansible collection for deploying LiteMaaS (AI Model Gateway) on OpenShift.

## Features

- **Cloud Agnostic**: Auto-detects AWS, Azure, GCP, CNV/ODF, vSphere, bare metal
- **RHPDS Ready**: Deploys to `rhpds` namespace following RHDP standards
- **Zero Config**: Works out of the box with auto-detection
- **Secure**: All secrets auto-generated using Ansible password lookup

## Supported Platforms

| Cloud Provider | Storage Class | Status |
|----------------|---------------|---------|
| AWS | gp3-csi / gp2-csi | ✅ Tested |
| Azure | managed-premium | ✅ Supported |
| GCP | standard-rwo | ✅ Supported |
| CNV with ODF | ocs-storagecluster-ceph-rbd | ✅ Supported |
| vSphere | thin-csi | ✅ Supported |
| Bare Metal | OCS/ODF | ✅ Supported |

## Quick Start

### Installation

```bash
# Clone the collection
git clone https://github.com/prakhar1985/rhpds.litemaas.git
cd rhpds.litemaas

# Build and install
ansible-galaxy collection build
ansible-galaxy collection install rhpds-litemaas-*.tar.gz --force

# Deploy
ansible-playbook rhpds.litemaas.deploy_litemaas
```

### Direct Usage (Development)

```bash
# Clone and use directly
git clone https://github.com/prakhar1985/rhpds.litemaas.git
cd rhpds.litemaas
ansible-playbook playbooks/deploy_litemaas.yml
```

## What Gets Deployed

All components are deployed to the `rhpds` namespace:

- **PostgreSQL**: Database (StatefulSet with PVC)
- **LiteMaaS Backend**: API server (Deployment)
- **LiteMaaS Frontend**: React UI (Deployment + Route)
- **LiteLLM Gateway**: AI model gateway (Deployment + Route)
- **OAuth Client**: OpenShift authentication (cluster-scoped)

## Access URLs

After deployment:

- Frontend: `https://litemaas-rhpds.<cluster-domain>`
- LiteLLM Admin: `https://litellm-rhpds.<cluster-domain>`

Credentials saved to: `~/litemaas-deployment-info.txt`

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `ocp4_workload_litemaas_namespace` | `rhpds` | Deployment namespace |
| `ocp4_workload_litemaas_version` | `0.1.2` | LiteMaaS version |
| `ocp4_workload_litemaas_cloud_provider` | auto-detect | Cloud provider |
| `ocp4_workload_litemaas_postgres_storage_class` | auto-detect | Storage class |
| `ocp4_workload_litemaas_postgres_storage_size` | `10Gi` | PVC size |

See `roles/ocp4_workload_litemaas/defaults/main.yml` for all variables.

## Examples

### Basic Deployment

```bash
ansible-playbook playbooks/deploy_litemaas.yml
```

### Custom Storage

```bash
ansible-playbook playbooks/deploy_litemaas.yml \
  -e ocp4_workload_litemaas_postgres_storage_size=50Gi
```

### Remove Deployment

```bash
ansible-playbook playbooks/deploy_litemaas.yml \
  -e ocp4_workload_litemaas_remove=true
```

## Prerequisites

- OpenShift 4.12+
- Ansible 2.15+
- `kubernetes.core` collection
- Cluster admin or project admin permissions

## Testing on RHDP Sandbox

```bash
# SSH to bastion
ssh lab-user@bastion.xxxxx.sandboxXXXX.opentlc.com

# Activate Python virtualenv
source /opt/virtualenvs/k8s/bin/activate

# Clone and deploy
git clone https://github.com/prakhar1985/rhpds.litemaas.git
cd rhpds.litemaas
ansible-playbook playbooks/deploy_litemaas.yml
```

## Status

🚧 **Work in Progress** - Currently testing on AWS

## Author

**Prakhar Srivastava**
Manager, Technical Marketing - Red Hat Demo Platform
Red Hat

## License

MIT
