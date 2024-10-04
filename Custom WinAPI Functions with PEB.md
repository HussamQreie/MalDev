# Lab: Custom WinAPI Functions with PEB

---

## 1. Introduction
In this lab, you'll create custom functions to retrieve system and process information without directly using standard Windows APIs. Instead, you'll use the Process Environment Block (PEB), providing an alternative approach for obtaining commonly accessed data, such as directory paths, environment variables, and process/thread IDs.

---

## 2. Fetch Directory Functions using PEB
Using the PEB, retrieve directory paths such as:

- **Temp folder path:** Typically `C:\Windows\Temp`
- **Windir folder path:** Typically `C:\Windows`
- **AppData folder path:** Typically `C:\Users\username\AppData`

**Task:**
- Write a function to retrieve each directory path via PEB instead of using standard APIs like `GetTempPathW` or `GetWindowsDirectoryW`.

---

## 3. Fetch System Processors Function
This function will serve as a partial replacement for `GetSystemInfo`, retrieving the number of processors available on the machine from the PEB.

**Task:**
- Write a custom function to fetch the number of processors by accessing the PEB instead of using `GetSystemInfo`.

---

## 4. Fetch Current Directory & Command Line
- **Current Directory Path:** Replacing `GetCurrentDirectory`
- **Process Command Line:** Replacing `GetCommandLine`

**Task:**
- Implement functions that fetch the current directory and the command line of the running process by accessing the PEB.

---

## 5. Fetch PID & TID
You will retrieve the Process ID (PID) and Thread ID (TID) of the current process and thread:

- **PID:** Replacing `GetCurrentProcessId`
- **TID:** Replacing `GetCurrentThreadId`

**Task:**
- Write functions to fetch the PID and TID by directly accessing the PEB instead of using the Windows API.

---

## 6. Environment Variables
PEB also stores environment variables for the current process.

**Task:**
- Create functions to access and enumerate the environment variables from the PEB.
- Implement a `PrintAllEnvValues()` function to display all the environment variables of the current process.

---

## 7. PEB & Environment Variables
Using Windbg, investigate the environment variables stored in the PEB.

**Task:**
- Run the `!peb` command in Windbg to see how environment variables are stored and managed.
- Compare the output of `!peb` with the results from your `PrintAllEnvValues()` function.

---

## 8. Enumerating PEB’s Environment Variables (_RTL_USER_PROCESS_PARAMETERS)
You will dive into the `_RTL_USER_PROCESS_PARAMETERS` structure within the PEB, which stores information about environment variables, among other things.

**Task:**
- Implement functions to enumerate and display the environment variables stored within this structure.

---

## 9. Utilizing Environment Variables
Create custom functions to retrieve paths from environment variables:

- **GetTmpPath:** Replacement for `GetTempPathW`, used to retrieve the path of the "Temp" directory.
- **GetAppDataPath:** Retrieve the path of the "AppData" directory.
- **GetWinDirPath:** Replacement for `GetWindowsDirectoryW`, used to retrieve the path of the "Windir" directory.
- **GetNumberOfProcessors:** Retrieve the number of processors in the system.

---

## 10. Replacing `GetCurrentDirectory`
You'll implement a custom version of the `GetCurrentDirectory` API, using the `CURDIR` structure.

**Task:**
- Create a `GetCurrentDir` function that mimics `GetCurrentDirectory` by accessing the PEB.

---

## 11. Replacing `GetCommandLine`
Replace the `GetCommandLine` API by accessing the PEB to retrieve the process command line.

---

## 12. Replacing `GetCurrentProcessId`
Create a custom function to replace `GetCurrentProcessId`. Address both 64-bit and 32-bit systems.

**Task:**
- Implement `_GetCurrentProcessId` for 64-bit and 32-bit systems.

---

## 13. Replacing `GetCurrentThreadId`
Create a custom function to replace `GetCurrentThreadId`. Address both 64-bit and 32-bit systems.

**Task:**
- Implement `GetCurrentThreadId` for 64-bit and 32-bit systems.

---

## Lab Summary
In this lab, you will gain experience working with the PEB to replace common Windows API functions, providing insight into how information is managed internally in the operating system. You'll also learn how to use Windbg to investigate the PEB and environment variables, as well as how to implement custom functions that interact directly with system-level data structures.
