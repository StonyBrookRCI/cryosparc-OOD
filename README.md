# CryoSPARC Open OnDemand App

This [Open OnDemand (OOD)](https://www.openondemand.org/) interactive app launches a full [**CryoSPARC**](https://guide.cryosparc.com/) interactive session on HPC compute nodes using Slurm. The app provides browser-based access to CryoSPARC services while all computation runs securely on allocated compute resources.

The app is designed for multi-user HPC environments and follows Open OnDemand best practices for interactive scientific applications.

## What this app provides

- A point and click OOD form to request CPUs, memory, GPUs, and walltime.
- Automated startup of CryoSPARC master and worker services inside a Slurm job.
- Secure browser access via SSH tunneling.
- Per-user CryoSPARC database and scratch locations.
- Integration with existing CryoSPARC installations.

## Directory layout

- `manifest.yml`. OOD app metadata.
- `form.yml.erb`. User-facing form (partition, CPUs, GPUs, memory, walltime, CryoSPARC paths).
- `submit.yml.erb`. Slurm submission configuration.
- `template/before.sh.erb`. Environment setup and runtime directory preparation.
- `template/script.sh.erb`. Starts CryoSPARC master and worker processes.
- `template/after.sh.erb`. Generates connection metadata for the view.
- `view.html.erb`. Displays SSH tunnel command and access URL.
- `icon.png`. App icon.

## Prerequisites

### Open OnDemand requirements

- Open OnDemand with **Batch Connect** enabled.
- Functional **Slurm** integration.
- Users must be able to SSH to compute nodes.

### Software requirements on compute nodes

- CryoSPARC installed and licensed.
- Supported Linux OS per CryoSPARC requirements.
- `python3`, `bash`, `curl`, and standard Unix utilities.
- NVIDIA GPUs and drivers if GPU acceleration is enabled.

### Storage requirements

The app requires writable directories for:

- CryoSPARC database.
- CryoSPARC scratch space.
- CryoSPARC software installation.

Paths are configurable in the OOD form and should point to high-performance storage.

## How it works

1. **before.sh**
   - Validates required directories.
   - Sets environment variables for CryoSPARC.
   - Prepares runtime paths.

2. **script.sh**
   - Loads CryoSPARC environment.
   - Starts the CryoSPARC master service.
   - Launches CryoSPARC workers based on requested resources.
   - Waits for services to become available.

3. **after.sh** and **view.html.erb**
   - Generates connection details.
   - Displays SSH tunneling instructions and access URL.

## Deployment guide for another HPC

### 1. Install the app

Copy the app directory to:

- `/var/www/ood/apps/sys/cryosparc`

Ensure correct permissions for the OOD web server.

### 2. Update cluster configuration

- Modify `cluster` in `form.yml.erb` to match your OOD cluster name.
- Update Slurm partitions, GPU options, and walltime limits.

### 3. Configure CryoSPARC paths

In `form.yml.erb` and `script.sh.erb`, set:

- CryoSPARC install directory.
- Database directory.
- Scratch directory.

### 4. Licensing

Ensure CryoSPARC licensing is configured system-wide and accessible on compute nodes.

### 5. Networking and security

- All services bind to compute nodes.
- Access is restricted via SSH tunneling.

## Troubleshooting

- **Service does not start**. Check Slurm job output and CryoSPARC logs.
- **Cannot connect**. Verify SSH tunneling and firewall settings.
- **Worker startup fails**. Confirm GPU availability and driver compatibility.

## Developed by

- [Arshad Mehmood](https://rci.stonybrook.edu/HPC/team/arshad-mehmood)
- [David Carlson](https://rci.stonybrook.edu/HPC/team/david-carlson)


## Help and support

- Arshad Mehmood. arshad.mehmood@stonybrook.edu
- David Carlson. david.carlson@stonybrook.edu
- [SBU Research Computing & Informatics (RCI) team](https://rci.stonybrook.edu/our-team)
