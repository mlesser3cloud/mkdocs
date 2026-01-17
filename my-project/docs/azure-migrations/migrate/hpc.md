# High Performance Computing Migration Strategy for Azure

A successful HPC migration to Azure requires careful planning across infrastructure, workloads, and deployment models. Azure supports multiple migration approaches ranging from hybrid cloud bursting to full cloud-native deployments.

## Migration Approaches

### Lift-and-Shift (Rehost)
Moving applications to Azure with minimal changes provides the quickest migration path. This approach uses Azure CycleCloud to replicate on-premises HPC environments with familiar schedulers like Slurm, PBS Pro, or LSF without modifying existing workflows.

### Hybrid Cloud Bursting
This model extends on-premises clusters into Azure to handle peak demands without maintaining excess local capacity. Jobs run locally during normal operations and automatically burst to Azure cloud resources when additional compute capacity is needed.

### Cloud-Native
A fully cloud-based deployment leverages Azure's native autoscaling, managed services, and orchestration capabilities. This approach maximizes cloud benefits but requires rearchitecting applications to utilize cloud-native features.

## Azure HPC Service Selection

| Service | Best For | Scheduler | Use Case |
|---------|----------|-----------|----------|
| **Azure CycleCloud** | Teams with existing HPC environments and scheduler preferences | Slurm, OpenPBS, PBS Pro, LSF, Grid Engine, custom schedulers | Organizations wanting complete HPC environment control with familiar schedulers |
| **Azure Batch** | Developers building cloud-native parallel workloads | Built-in Batch APIs and tools | Large-scale parallel processing without managing cluster infrastructure |
| **Azure HPC Pack** | Windows or Linux HPC workloads | HPC Pack scheduler | Organizations using Microsoft HPC Pack on-premises |

## Migration Workflow

### Phase 1: Assessment and Planning
- Conduct inventory of current HPC infrastructure including CPU, memory, disk, and networking specifications
- Define migration goals such as cost reduction, performance improvement, scalability, or innovation
- Verify Azure subscription quota limits accommodate target VM sizes for HPC workloads
- Identify appropriate Azure VM sizes including specialized HPC and GPU instances with high-speed networking for MPI workloads
- Select network topology following Azure HPC networking best practices

### Phase 2: Proof of Concept
Start with a proof-of-concept by provisioning a simple cluster in Azure using Azure CycleCloud with one familiar scheduler. This allows teams to understand Azure technology, assess application functionality, and investigate performance-cost tradeoffs compared to on-premises environments.

### Phase 3: Data Migration Strategy
- Use Azure HPC Cache for read-heavy file access workflows supporting up to 75,000 cores
- Consider partnering with Azure HPC industry specialists for large datasets with latency requirements
- Implement NFS-accessible storage or Azure Blob storage for temporary cloud processing
- Deploy parallel file systems like BeeGFS for high-throughput requirements

### Phase 4: Infrastructure Deployment
- Automate deployment using Azure Resource Manager templates for repeatability
- Deploy CycleCloud as a management server on an Azure VM with outbound access to Azure Resource Provider APIs
- Configure autoscaling plugins integrated with your chosen scheduler
- Set up authentication using Microsoft Entra ID for single sign-on capabilities
- Integrate monitoring with Azure Monitor and Prometheus/Grafana dashboards

### Phase 5: Workload Migration
- Begin with non-critical workloads to gain experience before migrating mission-critical applications
- Test application performance and validate results against on-premises baselines
- Optimize VM selection and autoscaling policies based on workload patterns
- Implement cost controls and governance policies for resource management

### Phase 6: Optimization and Scaling
- Continually reassess infrastructure requirements to improve performance and reduce costs
- Leverage Azure Cost Management tools to track and optimize spending
- Scale compute resources automatically based on job queue demands using CycleCloud autoscaling
- Consider moving suitable workloads to more cost-effective non-HPC clusters where appropriate

## Detailed Migration Process

### Step 1: Basic Infrastructure Setup

#### 1.1 Resource Group Configuration
- Navigate to Azure Portal and create a dedicated resource group for HPC resources
- Select appropriate Azure region based on latency requirements and service availability
- Establish naming conventions following organizational standards
- Configure tags for cost tracking, ownership, and environment classification

#### 1.2 Network Infrastructure
- Create a Virtual Network (VNet) with appropriate CIDR block for expected growth
- Design subnet topology including separate subnets for management, compute, and storage
- Configure Network Security Groups (NSGs) with rules for:
  - SSH/RDP access to management nodes
  - Scheduler communication ports
  - MPI communication for HPC workloads
  - Storage access protocols (NFS, SMB, Lustre)
- Enable Azure ExpressRoute or VPN Gateway for hybrid connectivity if required
- Configure DNS settings for name resolution between on-premises and Azure

#### 1.3 Identity and Access Management
- Configure Azure Active Directory integration for centralized authentication
- Implement Role-Based Access Control (RBAC) with principle of least privilege
- Create service principals for automated deployments and CycleCloud operations
- Set up Managed Identities for Azure resources to access other services securely
- Configure Multi-Factor Authentication (MFA) for administrative accounts

#### 1.4 Quota Verification and Increase
- Review current Azure subscription quota limits for target VM families
- Request quota increases for:
  - HPC VM series (HB, HBv2, HBv3, HC, ND, NC)
  - vCPU limits per region
  - Public IP addresses
  - Network interfaces
- Allow 3-5 business days for quota increase approval
- Document approved quotas for capacity planning

### Step 2: Storage Infrastructure Deployment

#### 2.1 Storage Architecture Selection
Choose appropriate storage solutions based on workload requirements:

**For High-Performance Parallel Workloads:**
- Deploy Azure Managed Lustre for applications requiring maximum throughput and low latency
- Configure Lustre file systems through Azure Marketplace
- Set up Object Storage Targets (OSTs) and Metadata Targets (MDTs)
- Establish appropriate stripe count and stripe size for workload patterns

**For Enterprise NFS Workloads:**
- Provision Azure NetApp Files for enterprise-grade performance
- Configure service levels (Standard, Premium, Ultra) based on IOPS requirements
- Set up snapshots and replication policies for data protection
- Create volumes with appropriate protocols (NFSv3, NFSv4.1, SMB)

**For General-Purpose Storage:**
- Create Azure Storage Accounts with NFS v3 support
- Enable hierarchical namespace for Azure Data Lake Storage Gen2 if needed
- Configure blob storage tiers (Hot, Cool, Archive) for cost optimization

#### 2.2 Storage Configuration Steps

**Azure Managed Lustre Setup:**

1. Navigate to Azure Marketplace and search for "Managed Lustre"
2. Configure file system parameters:
   - File system name and capacity (minimum 48 TiB)
   - Throughput per TiB setting
   - Subnet assignment
3. Deploy and wait for provisioning completion (typically 15-30 minutes)
4. Obtain mount instructions from the resource overview

**Azure NetApp Files Configuration:**

1. Create NetApp Account in target resource group
2. Establish capacity pool with desired service level
3. Create volumes specifying:
   - Volume quota and performance tier
   - Protocol type (NFS/SMB)
   - Export policies and mount paths
4. Obtain mount information from volume properties

#### 2.3 Client Configuration
On each HPC compute node:

1. Install required client packages:
   ```bash
   # For NFS
   sudo yum install nfs-utils -y  # RHEL/CentOS
   sudo apt-get install nfs-common -y  # Ubuntu
   
   # For Lustre
   sudo yum install lustre-client -y
   ```

2. Configure mount points in `/etc/fstab`:
   ```bash
   # Example NFS mount
   <storage-ip>:/<export-path> /mnt/data nfs defaults 0 0
   
   # Example Lustre mount
   <mgs-ip>@tcp:/<filesystem-name> /lustre lustre defaults 0 0
   ```

3. Create mount directories and mount filesystems:
   ```bash
   sudo mkdir -p /mnt/data
   sudo mount -a
   sudo chmod 755 /mnt/data
   ```

4. Verify mount success and performance:
   ```bash
   df -h
   mount | grep /mnt/data
   ```

### Step 3: Data Migration

#### 3.1 Data Migration Strategy Selection

**For Large Datasets (>10 TB):**
Use Azure Data Box for offline transfer:

1. Navigate to Azure Portal and order Azure Data Box
2. Select appropriate Data Box type:
   - Data Box Disk (up to 35 TB)
   - Data Box (up to 80 TB)
   - Data Box Heavy (up to 770 TB)
3. Specify target storage account and shipping address
4. Receive Data Box appliance at on-premises location
5. Connect Data Box and copy data using provided tools
6. Ship Data Box back to Azure datacenter
7. Verify data upload completion through Azure Portal
8. Validate data integrity using checksums

**For Medium Datasets (1-10 TB):**
Use AzCopy or Azure File Sync:

1. Install AzCopy tool on source system
2. Generate SAS token for target storage account
3. Execute parallel data transfer:
   ```bash
   azcopy copy '/source/path/*' 'https://<storage>.blob.core.windows.net/<container>/<SAS-token>' --recursive
   ```
4. Monitor transfer progress and resume if interrupted

**For Continuous Synchronization:**
Deploy Azure File Sync or rsync:

1. Install Azure File Sync agent on on-premises file servers
2. Register servers with Azure File Sync service
3. Create sync groups and cloud endpoints
4. Configure sync frequency and conflict resolution
5. For Linux environments, use rsync over ExpressRoute/VPN:
   ```bash
   rsync -avz --progress /source/data/ user@<azure-vm>:/mnt/data/
   ```

#### 3.2 Data Migration Validation
- Compare file counts and sizes between source and destination
- Verify file permissions and ownership preservation
- Test application access to migrated data
- Conduct performance benchmarking on migrated datasets
- Document migration completion and any issues encountered

### Step 4: CycleCloud Server Deployment

#### 4.1 CycleCloud VM Provisioning
1. Deploy CycleCloud from Azure Marketplace or manually:
   - VM Size: Standard_D4s_v3 or larger
   - Operating System: Ubuntu 20.04 LTS or RHEL 8.x
   - Attach managed disk for cluster configuration storage
   - Assign public IP if managing from external networks
   - Configure NSG to allow HTTPS (443) and SSH (22)

2. Install CycleCloud software (if manual deployment):
   ```bash
   wget -qO - https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   sudo apt-get install cyclecloud8 -y
   ```

3. Start CycleCloud service:
   ```bash
   sudo systemctl start cyclecloud
   sudo systemctl enable cyclecloud
   ```

#### 4.2 CycleCloud Initial Configuration
1. Access CycleCloud web interface at `https://<cyclecloud-ip>`
2. Accept self-signed certificate warning (or configure proper SSL certificate)
3. Create CycleCloud administrator account with strong password
4. Configure Azure credentials:
   - Subscription ID
   - Tenant ID
   - Application ID (Service Principal)
   - Application Secret
5. Validate credentials and verify Azure connectivity
6. Set default region and resource group for cluster deployments

#### 4.3 CycleCloud CLI Setup
On the CycleCloud server:

1. Initialize CycleCloud CLI:
   ```bash
   cyclecloud initialize
   ```

2. Provide configuration parameters:
   - CycleServer URL: `https://localhost`
   - Username: CycleCloud admin account
   - Password: Admin password
   - Accept untrusted certificate: Yes

3. Verify CLI configuration saved to `~/.cycle/config.ini`

4. Test CLI connectivity:
   ```bash
   cyclecloud show_cluster
   ```

### Step 5: HPC Cluster Deployment

#### 5.1 Cluster Template Selection
Choose appropriate cluster template based on scheduler preference:

- **Slurm**: Most common open-source scheduler, supports cloud bursting
- **Grid Engine**: Traditional HPC scheduler with extensive job control
- **PBS Pro**: Commercial-grade scheduler for complex workloads
- **LSF**: IBM Spectrum LSF for heterogeneous workloads
- **Custom**: Import custom cluster templates

#### 5.2 Cluster Configuration Parameters

**Basic Settings:**
- Cluster name and description
- Scheduler type and version
- Region and availability zones
- Virtual network and subnet selection

**Head Node Configuration:**
- VM size (typically Standard_D8s_v3 or larger)
- Operating system image (HPC images with pre-configured MPI libraries)
- Managed disk size for OS and local storage
- Public IP assignment (for external access)
- SSH public key or password authentication

**Compute Node Configuration:**
- VM size families (HBv3, HBv2, HC44rs for MPI workloads)
- Maximum node count per node array
- Autoscaling policies:
  - Scale-up threshold (queue depth)
  - Scale-down timeout (idle time)
  - Minimum and maximum node counts
- OS image selection (must match head node OS family)
- Spot instance usage for cost savings (if appropriate)

**Storage Configuration:**
- NFS mount points from head node
- External storage mounts (Lustre, NetApp Files)
- Shared filesystem size (100 GB default, increase as needed)
- Blob storage integration for backup (HSM functionality)

**Advanced Settings:**
- Cloud-init scripts for custom software installation
- Chef/Ansible integration for configuration management
- Cluster-init projects for software deployment
- Custom firewall rules and security configurations
- Monitoring and logging integrations

#### 5.3 Cluster Deployment Process

1. From CycleCloud web console, click **Clusters** > **+** (Add)
2. Select scheduler template (e.g., Slurm, PBS Pro)
3. Configure cluster parameters using wizard:
   - **About** tab: Name and description
   - **Required Settings**: Azure configuration, networking
   - **Compute**: VM sizes and autoscaling
   - **Storage**: NFS and external mounts
   - **Advanced**: Custom scripts and software
4. Review configuration summary
5. Click **Save** to create cluster definition
6. Click **Start** to begin deployment
7. Monitor deployment progress:
   - Scheduler node creation
   - Resource acquisition and IP assignment
   - Software configuration
   - Node template creation for compute nodes

Deployment typically takes 10-20 minutes for head node and initial setup.

#### 5.4 Post-Deployment Configuration

**Scheduler Configuration:**

1. SSH into head node using CycleCloud console or direct SSH
2. Verify scheduler service status:
   ```bash
   sudo systemctl status slurmctld  # For Slurm
   sudo systemctl status pbs  # For PBS Pro
   ```

3. Configure scheduler partitions/queues for different VM types:
   ```bash
   # Slurm partition example
   sudo scontrol create partition=hpc Nodes=hpc-[1-100] Default=YES
   ```

4. Set up job accounting and resource limits
5. Configure fair-share policies if needed

**User Account Configuration:**

1. Create user accounts on head node (synchronized to compute nodes)
2. Configure home directories on shared NFS storage
3. Set up SSH key-based authentication for users
4. Add users to appropriate groups for resource access

**Software Installation:**

1. Install compilers (GCC, Intel, PGI)
2. Deploy MPI libraries (OpenMPI, Intel MPI, MPICH)
3. Install HPC applications and dependencies
4. Configure environment modules (Lmod or Environment Modules)
5. Install monitoring tools (Ganglia, Prometheus, Grafana)

**Network Verification:**

1. Test inter-node communication using ping and SSH
2. Verify InfiniBand or high-speed networking (if applicable)
3. Test MPI communication with simple MPI jobs:
   ```bash
   mpirun -np 8 -hosts node1,node2 hostname
   ```

### Step 6: Workload Migration and Testing

#### 6.1 Test Job Submission
1. Create simple test job script:
   ```bash
   #!/bin/bash
   #SBATCH --job-name=test
   #SBATCH --nodes=2
   #SBATCH --ntasks-per-node=4
   #SBATCH --time=00:10:00
   
   echo "Job started on $(hostname)"
   srun hostname
   ```

2. Submit test job and monitor:
   ```bash
   sbatch test.sh
   squeue
   sacct -j <job-id>
   ```

3. Verify autoscaling triggers compute node deployment
4. Monitor node startup time and job execution
5. Confirm job completion and output files

#### 6.2 Application Validation
1. Install application software on shared storage
2. Transfer representative input datasets
3. Create application-specific job scripts
4. Run validation jobs and compare results with on-premises baseline
5. Measure performance metrics:
   - Job completion time
   - CPU utilization
   - Memory usage
   - Network throughput
   - Storage I/O performance

#### 6.3 Performance Benchmarking
Execute standard HPC benchmarks:

1. **HPL (High-Performance Linpack)** for computational performance
2. **IOR/mdtest** for storage performance
3. **OSU Micro-Benchmarks** for MPI latency and bandwidth
4. **STREAM** for memory bandwidth
5. Application-specific benchmarks

Document results and compare with on-premises performance targets.

#### 6.4 Workload Migration Schedule
1. Categorize workloads by priority and complexity
2. Begin with non-production development/test workloads
3. Migrate batch workloads before interactive jobs
4. Move production workloads in phases:
   - Week 1-2: Low-priority batch jobs
   - Week 3-4: Medium-priority workloads
   - Week 5-6: High-priority production workloads
   - Week 7+: Remaining specialty workloads
5. Maintain parallel on-premises and cloud operations during transition
6. Conduct user training and provide migration support

### Step 7: Monitoring and Optimization

#### 7.1 Monitoring Setup
1. Enable Azure Monitor integration for VM metrics
2. Configure CycleCloud monitoring dashboards
3. Deploy Prometheus and Grafana for detailed metrics
4. Set up alerting for:
   - Cluster health issues
   - Job failures
   - Autoscaling problems
   - Storage capacity warnings
   - Cost threshold breaches
5. Configure log aggregation using Azure Log Analytics

#### 7.2 Cost Optimization
1. Analyze job patterns to right-size VM selections
2. Implement Azure Spot VMs for fault-tolerant workloads
3. Configure aggressive autoscaling timeouts to minimize idle time
4. Use Azure Reserved Instances for baseline capacity
5. Implement automated shutdown schedules for non-production clusters
6. Monitor costs using Azure Cost Management
7. Set up budget alerts and spending limits

#### 7.3 Performance Tuning
1. Optimize VM placement groups for low-latency workloads
2. Enable accelerated networking on supported VM sizes
3. Tune scheduler parameters for autoscaling responsiveness
4. Optimize storage access patterns and caching
5. Implement data tiering strategies for infrequently accessed data
6. Review and adjust autoscaling policies based on actual usage
7. Continuously monitor and optimize application performance

#### 7.4 Security Hardening
1. Conduct security assessment using Azure Security Center
2. Implement network segmentation and microsegmentation
3. Enable Azure DDoS Protection if exposed to internet
4. Configure Azure Firewall for outbound filtering
5. Implement encryption at rest for all storage
6. Enable encryption in transit using TLS/SSL
7. Conduct regular vulnerability scanning and patching
8. Implement Azure Policy for governance enforcement
9. Enable audit logging for compliance requirements

### Step 8: Operational Handoff

#### 8.1 Documentation
Create comprehensive documentation covering:
- Architecture diagrams and network topology
- Cluster configuration parameters
- Storage layout and mount points
- User account management procedures
- Job submission guidelines and examples
- Troubleshooting guides and runbooks
- Disaster recovery and backup procedures
- Contact information for support escalation

#### 8.2 Training
Conduct training sessions for:
- **End Users**: Job submission, data management, best practices
- **Administrators**: Cluster management, troubleshooting, scaling
- **Application Owners**: Application deployment, optimization
- **Management**: Cost monitoring, capacity planning, governance

#### 8.3 Support Model
Establish support procedures:
- Tier 1: User support for basic job submission and access issues
- Tier 2: Application and performance support
- Tier 3: Infrastructure and Azure platform issues
- Escalation paths to Microsoft Azure support
- On-call rotation and incident response procedures

## Best Practices

- **Start small and scale gradually** by migrating pilot workloads before full-scale adoption
- **Leverage automation** through Azure Resource Manager templates and Infrastructure-as-Code approaches
- **Implement governance early** with policies for access control, cost management, and security
- **Use native Azure tools** including Azure Migrate for assessment and Azure Advisor for optimization recommendations
- **Plan for hybrid scenarios** if complete migration isn't feasible, enabling cloud bursting from on-premises to Azure
- **Enable comprehensive monitoring** using Azure Monitor integration and custom dashboards for cluster health
- **Continuously reassess infrastructure** requirements to improve performance and reduce costs
- **Validate at each step** before proceeding to ensure migration success and minimize risks
- **Maintain parallel operations** during transition to provide fallback capability
- **Document everything** to ensure knowledge transfer and operational sustainability
