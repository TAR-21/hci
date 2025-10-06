```markdown
# Minimum Resource Requirements for HCI Product Controllers

This document summarizes the minimum resource requirements for the controller, or its equivalent component, for major Hyper-Converged Infrastructure (HCI) products.

The "controller" in an HCI product plays a crucial role in managing functions like storage. Depending on the product, it may be implemented as a dedicated virtual machine (VM) or integrated into the hypervisor's kernel. Therefore, the approach to resource requirements differs for each product.

---

### ◆ Nutanix (Controller VM - CVM)

In a Nutanix environment, a virtual machine called the **Controller VM (CVM)** runs on each node to provide storage I/O and cluster management functions. A portion of the node's resources is allocated to the CVM.

| Resource        | Minimum Requirement                                   | Notes                                                                                             |
| :-------------- | :---------------------------------------------------- | :------------------------------------------------------------------------------------------------ |
| **vCPU** | 8 vCPU (4 cores)                                      | For a general-purpose configuration. The recommended value varies depending on the features used. |
| **Memory** | 32 GB                                                 | Enabling features like deduplication and compression may require more memory.                     |
| **Storage** | ~200GB+ for the CVM boot disk (SSD/NVMe recommended) | The CVM requires its own boot space, separate from the data disks.                                |

**Key Point:** The resources for the Nutanix CVM are essential for system stability. Since requirements change based on the AOS (Acropolis Operating System) version and configuration, it is important to use sizing tools to estimate the appropriate resources during deployment.

---

### ◆ VMware vSAN

VMware vSAN is directly integrated into the ESXi hypervisor kernel. Therefore, there is no independent controller VM like the Nutanix CVM. Resource requirements apply to **each ESXi host** participating in the vSAN cluster.

| Resource        | Minimum Requirement                                                                 | Notes                                                                                             |
| :-------------- | :---------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------ |
| **CPU** | -                                                                                   | vSAN itself consumes relatively little CPU, but the host CPU must be sufficient for VM workloads. |
| **Memory** | 32 GB                                                                               | For small-scale configurations. More memory is required depending on disk capacity per node.      |
| **Network** | 10 Gbps                                                                             | A minimum of 10 Gbps is mandatory for vSAN inter-node communication (e.g., data synchronization). |
| **Storage** | • Cache tier: 1+ SSD/NVMe<br>• Capacity tier: 1+ SSD or HDD                        | Each host needs disks for both a cache tier and a capacity tier.                                  |
| **Boot Device** | 8 GB+ USB/SD card, or a 32 GB+ local disk                                           | This is the boot device for the ESXi host.                                                        |

**Key Point:** vSAN's performance is heavily dependent on the hardware performance of each host, especially the network and storage devices.

---

### ◆ Microsoft Azure Stack HCI

Similar to vSAN, Azure Stack HCI does not have a separate controller VM. The functionality is integrated into the operating system, and the resource requirements apply to **each server node** that makes up the cluster.

| Resource          | Minimum Requirement (per node)                               | Notes                                                                                            |
| :---------------- | :----------------------------------------------------------- | :----------------------------------------------------------------------------------------------- |
| **CPU** | 64-bit processor (Intel Nehalem / AMD EPYC or newer)         | Must meet the execution requirements for Hyper-V.                                                |
| **Memory** | 32 GB                                                        | It is recommended to secure sufficient capacity to account for the system and VM workloads.      |
| **Network** | 10 Gbps or higher                                            | A high-speed network is recommended for storage synchronization traffic (Storage Spaces Direct). |
| **Storage (OS)** | 200 GB+ boot drive                                           | This is the space for the OS installation.                                                       |
| **Storage (Data)**| 2 or more SSDs or HDDs of 500 GB+                            | Required to configure Storage Spaces Direct (S2D).                                               |

**Key Point:** Azure Stack HCI is based on technologies like Windows Server Failover Clustering and Storage Spaces Direct, and it requires server resources that can stably run these features.

---

### ◆ Dell EMC VxRail

VxRail is an appliance product based on VMware vSAN. Therefore, its fundamental resource principles are aligned with vSAN. The controller functionality is integrated into the ESXi kernel, and resources are defined on a **per-VxRail node (appliance)** basis.

The minimum configuration starts from 2 or 3 nodes, but the specific CPU, memory, and storage specs for each node depend on the selected model.

- **Example of Memory:** Even entry-level models are equipped with **64 GB or more** of memory per node.

**Key Point:** Since VxRail is delivered as an integrated hardware and software package, users do not need to size the controller resources individually. Instead, they select a model optimized for their intended use case and scale.

---

### ## Summary

| Product             | Controller Type                 | CPU                  | Memory                     |
| :------------------ | :------------------------------ | :------------------- | :------------------------- |
| **Nutanix** | Controller VM (CVM)             | 8 vCPU (4 cores)+    | 32 GB+                     |
| **VMware vSAN** | Kernel-Integrated               | (Host-dependent)     | 32 GB+ (per host)          |
| **Azure Stack HCI** | OS-Integrated                   | (Host-dependent)     | 32 GB+ (per host)          |
| **Dell EMC VxRail** | Kernel-Integrated (vSAN-based)  | (Model-dependent)    | 64 GB+ (per node)          |

**Disclaimer:**
The minimum requirements for each product represent a baseline for operating basic functions. In a real-world deployment, it is crucial to secure sufficient resources by considering the workloads to be run, features to be used, and future scalability.
```
