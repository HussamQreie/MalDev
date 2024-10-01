# MalDev
This Repo is to learn MalDev on X64 architecture on both User and Kernel levels by Question-Answer technique
# ChatGpt input - Technique
```sh
how to do this lab this lab is guide to how binary metadata modification happened this helped it professional a lot .
ensure these steps  Open up Visual Studio and create a new solution,
  Add a New Item, Add a Resource (.rc) File, Exit Resource Viewer,
  After exiting the resource viewer, right-click the resource file and select 'Open With' > 'Source Code Text) Editor',
  Insert Metadata code,  Compile & View the Results,
 It's also possible to give the binary an icon, which again is a step towards making the binary appear less suspicious,
  .ico File Placement,  Modify The Resource File by add icon, Compile & View the Results.
```

#  Thread Enumeration - NtQuerySystemInformation

## Intro

These APIs used to  retrieve system information, why? from offensive security presepective? it may help in finding targets to inject into, hijack, or manipulate (for example, during process injection or thread hijacking), types of these APIs:

- **High-level APIs** (like CreateToolhelp32Snapshot, Thread32First, or Thread32Next): No, because they are easily detectable, especially if an EDR or AV is in use.
  
- **Low-level APIs (Used In this Lab)** (like NtQuerySystemInformation): Yes, if EDR or other advanced monitoring tools are not in use, but they are still potentially detectable due to userland hooks.  avoid syscalls that are frequently hooked.

- **Direct/indirect syscalls**: Highly recommended, especially when EDR or advanced security measures are in place, as they bypass userland hooks and reduce detection risk. Direct? call goes directly to the kernel. Indirect? using another trusted process to make the call.

---

## Understanding Windows SDK Headers, ntdll.lib, and NtQuerySystemInformation.

### Windows SDK Headers
- **Definition:** Windows SDK headers are a collection of C/C++ header files provided by the Windows Software Development Kit (SDK) that **define the application programming interfaces (APIs) for the Windows operating system.** These headers include declarations for functions, macros, data types, and structures necessary for developing applications that run on the Windows platform.

- **Purpose:** They enable developers to access and utilize the functionality provided by the Windows operating system, such as user interface elements, file handling, process management, and system-level operations.

- **Example:** Headers like windows.h, winternl.h(where our function), and winbase.h are commonly included in applications to leverage Windows APIs.

---

### ntdll.lib
- **Definition:** ntdll.lib is a static library that provides the import definitions for the functions implemented in the ntdll.dll (NT Layer DLL). It contains declarations for **low-level Windows API functions**, many of which are used for system and kernel-level operations.

- **Purpose:** The library allows developers to link their applications with the ntdll.dll, enabling access to a variety of functions that interact directly with the Windows kernel, including advanced features like thread management, memory operations, and system information retrieval.

- **Example:** Functions like NtQuerySystemInformation, NtCreateThread, and NtTerminateProcess are commonly accessed through ntdll.lib to perform low-level tasks in Windows applications.

---

### NtQuerySystemInformation
- **Definition:** NtQuerySystemInformation is a low-level Windows API function used **to retrieve various types of system-related information from the operating system.** This function provides detailed information about the system’s processes, threads, memory usage, and other critical data structures that are essential for system monitoring and management tasks.

- **Purpose:** The function allows developers to access information that may not be available through higher-level Windows APIs. It can be utilized for tasks such as:
Enumerating system processes and threads.
Gathering statistics on system performance.
Monitoring system resource usage.
Conducting forensic analysis or troubleshooting system issues.

- **Parameters:**
The function takes four parameters:
  * **SystemInformationClass:** Specifies the category of information to retrieve (e.g., processes, threads, etc.).
  * **SystemInformation:** A pointer to the buffer where the requested information will be stored.
  * **SystemInformationLength:** The size of the buffer pointed to by SystemInformation, in bytes.
  * **ReturnLength:** A pointer to a variable that receives the size of the information returned.

- **Return Value:**
The function returns an NTSTATUS code indicating the success or failure of the operation. Common return values include:
STATUS_SUCCESS: Indicates that the function completed successfully.
STATUS_INFO_LENGTH_MISMATCH: Indicates that the buffer provided is too small for the requested information.

- **Example Usage:**
NtQuerySystemInformation is often used in system-level applications, performance monitoring tools, and security software to gain insights into the operating system’s behavior and resource usage.





---

## Our Goal

-  How to implement thread enumeration using NtQuerySystemInformation, using Visual Studio and Windows SDK.

---
## Offensive Security Lab: Thread Enumeration using `NtQuerySystemInformation`

### Tools Required
- **Visual Studio**
- **Windows SDK**

The steps will cover:

- SYSTEM_PROCESS_INFORMATION Structure
- SYSTEM_THREAD_INFORMATION & CLIENT_ID Structures
- The Implementation
- ListRemoteProcessThreads Function
- GetRemoteProcessThreads Function

### Why Visual Studio and Windows SDK?

1. **Visual Studio**:
   - Comprehensive support for Windows development with project templates, debugging tools, and IntelliSense for autocompletion.
   - Excellent debugging features: inspect variables, set breakpoints, and step through code.
   - Integrates well with the Windows SDK to access low-level Windows APIs like `NtQuerySystemInformation`.

2. **Windows SDK**:
   - Provides headers and libraries necessary for developing Windows-native applications.
   - Gives access to undocumented functions like `NtQuerySystemInformation`.

## Lab Setup: A Step-by-Step Guide

### 1. Install Visual Studio
- Download and install **Visual Studio** from the [official website](https://visualstudio.microsoft.com/).
- Select the **Desktop development with C++** workload during installation.

### 2. Install Windows SDK
- The Windows SDK is typically included with Visual Studio, but if not, download it from [Microsoft Developer](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk/).

### 3. Create a New Project in Visual Studio
1. Open **Visual Studio**.
2. Select **Create a new project**.
3. Choose **Console** under C++ templates.
4. Name the project (e.g., `ThreadEnumerationLab`) and click **Create**.

### 4. Configure Project Settings

- Go to **Project > Properties** to configure settings.
   - Under **C/C++ > General > Additional Include Directories**, add the directory for the Windows SDK headers.
     *  **C:\Program Files (x86)\Windows Kits\10\Include\10.0.22621.0'** default location of Windows SDK headers. 10.0.22621.0 is e.g. you may have differnet version but it would be like this.

   - Under **Linker > Input > Additional Dependencies**, add `ntdll.lib`.
     * by Insert **ntdll.lib;** 

### 5. Code Implementation: Offensive PoC for Thread Enumeration

Here is the implementation for enumerating threads of a target process using `NtQuerySystemInformation`.

#### 5.1. Include Required Libraries

```cpp
#include <windows.h>
#include <winternl.h>
#include <stdio.h>
#include <stdlib.h>
```
