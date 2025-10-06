# Minimum Resource Requirements for HCI Product Controllers

This document summarizes the minimum resource requirements for the controller, or its equivalent component, for major Hyper-Converged Infrastructure (HCI) products. The controller plays a crucial role in managing functions like storage. Depending on the product, it may be a dedicated virtual machine (VM) or integrated into the hypervisor's kernel.

---

## Nutanix (Controller VM - CVM)

On each Nutanix node, a **Controller VM (CVM)** runs to manage storage I/O and cluster functions. A portion of the node's resources is dedicated to the CVM.

| Resource | Minimum Requirement | Notes |
| :-- | :-- | :-- |
| **vCPU** | 8 vCPU (4 cores) | The recommendation varies based on features used. |
| **Memory** | 32 GB | Enabling features like deduplication may require more memory. |
| **Storage** | ~200 GB+ boot disk (SSD/NVMe recommended) | The CVM requires its own boot space, separate from data disks. |

**Key Point:** CVM resources are essential for system stability. Requirements change based on the AOS (Acropolis Operating System) version, so always use official sizing tools for deployment.

---

## VMware vSAN

VMware vSAN is integrated directly into the ESXi hypervisor kernel, so there is no separate controller VM. Requirements apply to **each ESXi host** in the cluster.

| Resource | Minimum Requirement | Notes |
| :-- | :-- | :-- |
| **CPU** | - | Host CPU must be sufficient for all VM workloads. |
| **Memory** | 32 GB | More is required depending on the disk capacity per node. |
| **Network** | 10 Gbps | Mandatory for vSAN inter-node communication (e.g., data sync). |
| **Storage (Cache Tier)** | 1 or more SSDs/NVMe | Each host requires a cache tier. |
| **Storage (Capacity Tier)** | 1 or more SSDs or HDDs | Each host requires a capacity tier. |
| **Boot Device** | 8 GB+ USB/SD card, or 32 GB+ local disk | This is the boot device for the ESXi host itself. |

**Key Point:** vSAN performance depends heavily on the host's hardware, especially the network and storage devices.

---

## Microsoft Azure Stack HCI

Similar to vSAN, Azure Stack HCI integrates its control functions into the operating system on **each server node** in the cluster.

| Resource | Minimum Requirement (per node) | Notes |
| :-- | :-- | :-- |
| **CPU** | 64-bit processor (Intel Nehalem / AMD EPYC+) | Must meet requirements for Hyper-V. |
| **Memory** | 32 GB | Secure enough capacity for the OS and all VM workloads. |
| **Network** | 10 Gbps or higher | Recommended for storage sync traffic (Storage Spaces Direct). |
| **Storage (OS)** | 200 GB+ boot drive | Space for the OS installation. |
| **Storage (Data)** | 2 or more SSDs or HDDs of 500 GB+ | Required to configure Storage Spaces Direct (S2D). |

**Key Point:** Azure Stack HCI relies on Windows Server technologies and requires robust server resources for stable operation.

---

## Dell EMC VxRail

VxRail is an appliance based on VMware vSAN, so its resource principles are similar. Controller functions are integrated into the ESXi kernel on **each VxRail node**.

- **Specs:** The specific CPU, memory, and storage specs depend on the appliance model chosen.  
- **Memory Example:** Even entry-level models are equipped with **64 GB or more** of memory per node.

**Key Point:** As a pre-integrated system, VxRail does not require users to size controller resources manually. You select a model that is optimized for your specific workload and scale.

---

## Summary

| Product | Controller Type | CPU | Memory |
| :-- | :-- | :-- | :-- |
| **Nutanix** | Controller VM (CVM) | 8 vCPU (4 cores)+ | 32 GB+ |
| **VMware vSAN** | Kernel-Integrated | (Host-dependent) | 32 GB+ (per host) |
| **Azure Stack HCI** | OS-Integrated | (Host-dependent) | 32 GB+ (per host) |
| **Dell EMC VxRail** | Kernel-Integrated (vSAN-based) | (Model-dependent) | 64 GB+ (per node) |

**Disclaimer:** These minimums are a baseline. Real-world deployments require sufficient resources to handle your specific workloads, enabled features, and future growth.
