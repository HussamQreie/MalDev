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

