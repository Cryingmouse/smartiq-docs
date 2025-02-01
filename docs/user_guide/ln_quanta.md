# **LN-Quanta: Hardware/Software Abstraction Library**

## **Overview**
**LN-Quanta** is the library developed by LenovoNetapp, which designed to decouple hardware dependencies and software 
management logic. It provides standardized interfaces for operations, enabling developers to interact with 
heterogeneous systems uniformly.

---

## **Core Features**

### 1. **Hardware & System Information Collection**  
- **Unified Inventory**:  
  - **OS & Kernel**: Linux distribution version (e.g., RHEL, Ubuntu, CentOS), kernel release, and architecture (x86_64, ARM).
  - **Hardware Inventory**:  
    - **Storage**: Disk models (HDD/SSD/NVMe), RAID controller details (MegaRAID, PERC), filesystem types (ext4, XFS).  
    - **Network**: NIC vendor/model (Intel X710, Broadcom NetXtreme), driver versions, bond/team configurations, IP addresses, routing tables, etc.  
    - **Compute**: CPU details (Intel Xeon, AMD EPYC), core topology, DIMM slot population, BIOS/firmware versions.

  - **Multi-Hardware Platform Support**:  
    - **Enterprise Servers**:
      - **Lenovo**: ThinkSystem SR650/SR850.
      - **Custom OEM**: Whitebox servers with SMBIOS/DMI compliance.
    - **Hypervisor Hosts**:
      - VMWare ESXi (via ***esxcli*** passthrough on Linux-based management VMs).
      - KVM/QEMU hosts with PCIe passthrough device detection.

- **Linux-Centric Design**:  
  - **Native Integration**: Leverages Linux-specific interfaces (`sysfs`, `/proc`, `lsblk`, `lspci`, `dmidecode`) for granular hardware insights.  
  - **Extended OS Support**: Optional plugins for BSD variants (FreeBSD, OpenBSD) with reduced feature scope.  

### 2. **Configuration File Parsing**
- **Structured Output**: Parse `/etc/os-release`, `ceph.conf`, etc. into JSON/YAML.
- **Custom Parsers**: Extensible engine for proprietary config formats.

### 3. **System Command Abstraction**
- **Platform-Agnostic APIs**:
    - Service control: `start_service(name)`, `stop_service(name)`.
    - Network management: `set_ip_address(interface, ip)`.
- **Error Normalization**: Convert OS-specific errors to standard exceptions (e.g., `DeviceNotFoundError`).

## **Architecture**

| **Layer**     | **Technologies** | **Responsibilities**                                                         |
|---------------|------------------|------------------------------------------------------------------------------|
| **CLI**       | `click`          | User interaction, input validation, and help documentation.                  |
| **Interface** | `pluggy`         | Define abstract contracts (e.g., `get_smart_info()`, `list_block_device()`). |
| **Driver**    | `pluggy plugins` | Platform-specific logic.                                                     |
| **Libray**    | `System APIs`    | Low-level command execution, file I/O, parser, etc.                          |

### Command Line Interface

The CLI layer handles user interactions via a command-line interface, leveraging **`click`** to define commands, 
arguments, and options. It ensures input validation, generates help documentation, and formats output 
for usability. This layer delegates core logic to the Interface layer, acting as a bridge between user inputs 
and the application’s internal components. For example, commands like **`storops node get-info`** or configure 
trigger calls to the Interface layer’s abstract APIs (e.g., **`get_system_info()`**).

### Interface 

The Interface layer defines abstract contracts (e.g., **`get_system_info()`**, **`get_os_info()`**) using pluggy hooks, 
decoupling functionality from implementation. It includes two types of interfaces:

- **Monolithic Interface**: Directly declares platform-agnostic APIs (e.g., disk detection, configuration parsing) 
- that drivers must implement.

- **Complex Interface**: Composes monolithic interfaces to build higher-level workflows (e.g., automated disk 
partitioning based on parsed configurations). This layer ensures a consistent API surface for the CLI while 
enabling extensibility.

### Driver

Drivers implement the monolithic interfaces with platform-specific logic or commands. Each driver is responsible 
for translating abstract API contracts (e.g., **`get_system_info()`**) into concrete actions tailored to the target 
operating system or environment.

!!! note "Implementation Guidelines"

    - ***Single-command focus***: A monolithic interface should encapsulate **one specific command or operation** (e.g., querying disk information). Avoid bundling multiple unrelated actions into a single interface.
    - ***Structured parsing***: Raw output from system commands (e.g., **`lsblk`**) must be **parsed into structured data** with **`pydantic models`** (e.g., JSON, dataclass objects) before returning it to higher layers. Minimize exposing raw text or unstructured results.

### Library

The Library layer provides reusable, low-level utilities.

## **Get Started**

### **Installation**:

```shell
  pip install ln-quanta
```

### **Basic Usage**:

This tool provides two fundamental usage methods to meet hardware information retrieval needs in different scenarios:

#### 1. Command Line Interface (CLI)
```shell
  storops node get-info
```

#### 2. Python Library
```python
  from lnquanta.interface.disk import DiskInterface
  
  # Get cross-platform hardware info
  disk_interface = DiskInterface()
  print(disk_interface.list_block_device())
```

## **Code Structure Layer**
```
lnquanta/
├── command/                # CLI interface directory (click-based command definitions)
│
├── interface/              # Northbound interface declarations
│   ├── disk.py             # Disk interface
│   ├── ntp.py              # NTP interface
│   └── ...        
│  
├── driver/                 # Southbound interface implementations
│   ├── disk/               
│   │   └── list_block_device.py       # List block devices
│   │   └── partition_disk.py          # Partition disks
│   │   └── ...       
│   ├── ntp/               
│   │   └── get_ntp_config.py          # Get NTP configuration
│   │   └── get_ntp_tracking.py        # Get NTP tracking information
│   │   └── ...             
│   └──  ... 
│
├── lib/                    # Common utility encapsulations
│   └── hook.py             # Plugin hooks and decorators
│   └── parser/
│       └── dmidecode.py    # DMI decode parser
│       └── fstab.py        # Fstab file parser
│       └── ...   
│
├── main.py                 # CLI entry point
│
├── tests/                  # Unit tests
│
└── component_tests/        # Component tests (environment-dependent)
```

## **Technical term**
- **get**:     Query the specified resource information
- **list**:    List the resource information with the specified resource type
- **create**:  Create the new resource(s) in the storage cluster
- **delete**:  Delete the resource(s) in the storage cluster
- **set**:     Set up the resource(s) configuration in the storage cluster
- **update**:  Update some attributes of resource(s)
- **add**:     Add sub resource(s) to the attribute of resource(s).