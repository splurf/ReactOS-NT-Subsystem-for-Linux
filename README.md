# ReactOS-NT-Subsystem-for-Linux
A custom, micro-virtualized ReactOS NT kernel running as a subsystem for Windows compatibility on Linux (specifically interacting with Wine).

# 🚀 NT-Kernel-MicroVM-for-Wine (RSL)

| License | Status |
| :--- | :--- |
| **GPL-2.0** | **Proof-of-Concept** |

---

## 💡 Overview

The **NT-Kernel-MicroVM-for-Wine** project, or **ReactOS Subsystem for Linux (RSL)**, is a highly ambitious, clean-slate effort to solve the problem of **Windows NT kernel-level compatibility** on Linux.

The goal is to run a **minimal, stripped-down ReactOS kernel** inside a lightweight **Micro-VM** (like Firecracker or QEMU-microvm) and establish a fast, secure communication channel (a "Bridge") between it and the Linux host. This would allow the Linux environment (specifically the **Wine** compatibility layer) to dynamically offload low-level, kernel-dependent **NT API calls**—most importantly, the execution of **Windows device drivers**—to a correct, isolated NT environment.

This project aims to leverage modern virtualization techniques to provide the deepest possible Windows application and driver compatibility without full operating system virtualization.

---

## ⚖️ Licensing

This project is licensed under the **GNU General Public License, Version 2.0 (GPLv2)**.

This license is mandatory for several reasons:

1.  **ReactOS Compatibility:** The core components of the ReactOS kernel are primarily licensed under GPLv2-or-later. Reusing or integrating this code requires our derivative work to adopt a compatible copyleft license.
2.  **Linux Kernel Compatibility:** Any deep integration with the Linux host kernel (e.g., specific IPC drivers or modules) must be compliant with the **GPLv2-only** license of the Linux kernel.
3.  **Project Integrity:** The GPLv2 ensures that all derivatives and modifications of this project must also remain free and open source, preserving the project's goal of an open alternative to proprietary Windows compatibility solutions.

---

## 🛠️ Project Roadmap & Contribution Checklist

The project is divided into three major phases. Contributors are encouraged to select any available task and work on it independently.

### Phase 1: Isolation and Communication Foundation

**Goal:** Successfully boot a minimal NT kernel within a Micro-VM and establish basic data exchange with the Linux host.

| Task ID | Task Description | Status | Target Contributor Area |
| :--- | :--- | :--- | :--- |
| **P1-01** | **Select & Configure Micro-VM:** Choose and document the specific configuration for a lightweight hypervisor (e.g., Firecracker/QEMU microvm) to host the NT kernel. | ⚪ Open | Virtualization/DevOps |
| **P1-02** | **Minimal ReactOS Build:** Create a script/procedure to strip ReactOS down to only the NTOSKRNL, HAL, and NTDLL. | ⚪ Open | ReactOS Internals |
| **P1-03** | **Custom RSL Bootloader:** Develop a minimal boot shim that launches the stripped NT kernel and immediately initiates the IPC server, skipping user-mode setup. | ⚪ Open | Low-Level/Kernel Dev |
| **P1-04** | **Establish Host IPC Channel:** Implement a low-latency, kernel-mediated IPC mechanism (e.g., vSockets, shared memory) between the Linux user-space and the Micro-VM guest. | ⚪ Open | Linux Kernel/Systems |

### Phase 2: Core API Bridge Implementation

**Goal:** Implement the "Shim" and "Server" to correctly route and execute low-level NT system calls.

| Task ID | Task Description | Status | Target Contributor Area |
| :--- | :--- | :--- | :--- |
| **P2-01** | **Develop Linux-Side Shim:** Create a shared object (`libRSL.so`) to run on the Linux host that intercepts calls from Wine and serializes them for IPC transmission to the RSL. | ⚪ Open | Wine/Linux Userland |
| **P2-02** | **Develop RSL-Side Server:** Implement the component *inside* the NT kernel (or a minimal process) that receives the serialized IPC data and translates it into native NT Executive function calls. | ⚪ Open | ReactOS Internals |
| **P2-03** | **Design Serialization Protocol:** Define a stable protocol for transferring NT structure data and arguments across the IPC channel (e.g., for IRPs). | ⚪ Open | Protocol/Systems Design |
| **P2-04** | **PoC: `ZwClose` Implementation:** Implement a full round-trip for a single, non-trivial NT API call (e.g., `ZwClose`) to prove the bridge works. | ⚪ Open | Systems Programming |

### Phase 3: Driver and Wine Integration

**Goal:** Modify Wine to use the RSL and prove that a simple Windows driver can be loaded and interact with the Linux host I/O stack.

| Task ID | Task Description | Status | Target Contributor Area |
| :--- | :--- | :--- | :--- |
| **P3-01** | **Wine Integration Layer:** Patch selected Wine DLLs (like `ntdll`) to dynamically redirect hard-to-emulate NT calls to the `libRSL.so` shim. | ⚪ Open | Wine/Compatibility |
| **P3-02** | **I/O and Driver Management:** Implement the necessary logic in the RSL Server to manage I/O Request Packets (IRPs) and correctly pass I/O requests to the host Linux kernel's device model. | ⚪ Open | Kernel I/O / Driver Dev |
| **P3-03** | **PoC: Network Driver Load:** Successfully load a simple Windows network driver (e.g., a virtualized one) inside the RSL and verify its communication with the host network stack. | ⚪ Open | Networking/Driver Dev |
| **P3-04** | **Host Resource Management:** Implement control group (`cgroup`) or hypervisor configurations on the host to manage and throttle the Micro-VM resources. | ⚪ Open | Linux Systems/DevOps |

---

## 🧑‍💻 How to Contribute

We operate on a model of independent, goal-oriented development. If you see a task in the Roadmap marked **⚪ Open**, you are free to begin work on it.

1.  **Claim a Task:** Comment on the relevant GitHub Issue (or create a new one if the task is not yet listed) stating your intention to work on it.
2.  **Fork the Repository** and begin development.
3.  **Submit a Pull Request (PR):**
    * Ensure your PR title clearly references the Task ID (e.g., `[P2-01] Implemented Linux-Side Shim`).
        * Provide detailed notes on how your solution was tested and its impact on the system.
        4.  **License Compliance:** All contributions must be made under the **GPLv2.0** license.

        Every successful contribution moves the project closer to the common goal of advanced Windows application compatibility on Linux!
        