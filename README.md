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
## Binary MetaData Modification

### 1. Open Visual Studio and Create a New Solution
- Launch Visual Studio.
- Go to **File** > **New** > **Project**.
- Select **Console App** or any basic C++ project template (for simplicity).
- Choose a location to save your project and click **Create**.

### 2. Add a New Item
- In **Solution Explorer**, right-click the project name.
- Select **Add** > **New Item**.
- From the list, choose **Resource File (.rc)** and name it (e.g., `metadata.rc`).
- Click **Add**.

### 3. Exit Resource Viewer
- Once the resource file opens in Resource Viewer, close the viewer by simply closing the tab for the `.rc` file.

### 4. Open Resource File in Text Editor
- In **Solution Explorer**, right-click the resource file (`metadata.rc`).
- Select **Open With** > **Source Code (Text) Editor**. This will allow you to directly edit the resource file.

### 5. Insert Metadata Code
Now, insert the metadata code similar to the one you provided:

```c
1 VERSIONINFO
 FILEVERSION 112,0,5615,88
 PRODUCTVERSION 1,0,0,0
 FILEFLAGSMASK 0x0L

 #ifdef _DEBUG
  FILEFLAGS 0x1L
 #else
  FILEFLAGS 0x0L
 #endif

 FILEOS 0x4L // VOS__WINDOWS32
 FILETYPE 0x1L // VFT_APP
 FILESUBTYPE 0x0L // VFT2_UNKNOWN

 BEGIN
  BLOCK "StringFileInfo"
  BEGIN
   BLOCK "040904B0"
   BEGIN
    VALUE "CompanyName", "Google LLC."
    VALUE "FileDescription", "Google Chrome"
    VALUE "InternalName", "Chrome"
    VALUE "LegalCopyright", "Copyright 2023 Google LLC."
    VALUE "OriginalFilename", "chrome.exe"
    VALUE "ProductName", "Google Chrome"
    VALUE "ProductVersion", "112.0.5615.86"
   END
  END

  BLOCK "VarFileInfo"
  BEGIN
   VALUE "Translation", 0x409, 1200
  END
 END 
```
This metadata simulates a legitimate program like Google Chrome.

### 6. Compile & View the Results
- Once you’ve added the metadata, save the resource file.
- Compile the project by going to **Build** > **Build Solution**.
- You can inspect the compiled binary using a tool like **Resource Hacker** to see if the metadata has been embedded correctly.

### 7. Add an Icon to Make It Look Legitimate
- Download an appropriate icon in `.ico` format that you'd like to assign to the binary.
- Place the `.ico` file inside your project directory.

### 8. Modify the Resource File to Include the Icon
- Open the resource file (`metadata.rc`) again in **Source Code (Text) Editor**.
- Add the following code at the top of the file to include the icon:

```c
IDI_ICON1 ICON "youricon.ico"
```

### 9. Compile & View the Results (with Icon)
- Once you’ve added the icon reference, compile the project again.
- After the build completes, check the output binary. The binary should now have the custom icon associated with it.

### 10. Verify the Metadata and Icon
- Use **Resource Hacker** or **File Properties** on Windows to verify that the metadata and icon were successfully embedded into the binary.

## Tools for Verification
- **Resource Hacker**: This tool allows you to view and edit the embedded resources in executables.
- **File Properties**: Right-click the binary, select **Properties**, and check the **Details** tab to confirm the metadata is in place.

## Summary
This process enables you to embed metadata and icons into a binary, which can help make it appear legitimate. For professionals in cybersecurity, this type of metadata modification is essential to understand as it’s often used in malware to disguise files as legitimate software.

