# Windows Sandbox Config 


Windows Sandbox (WSB) offers a lightweight, isolated desktop environment for safely running applications. It's ideal for testing, debugging, exploring unknown files, and experimenting with tools.

This is a document outlines how .wsb (config files) can be used to create a customisable windows sandbox environment. 

Further details can be found Microsoft's [Instructional documentation](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/windows-sandbox-configure-using-wsb-file#create-a-configuration-file). Other examples can be found [here](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/windows-sandbox-sample-configuration)

#### Example .wsb config 

```XML
<Configuration>
  <VGpu>Disable</VGpu>
  <Networking>Disable</Networking>
  <MappedFolders>
    <MappedFolder>
      <HostFolder>C:\Users\Public\Downloads</HostFolder>
      <SandboxFolder>C:\temp</SandboxFolder>
      <ReadOnly>true</ReadOnly>
    </MappedFolder>
  </MappedFolders>
  <LogonCommand>
    <Command>explorer.exe C:\temp</Command>
  </LogonCommand>
</Configuration>
```

>*This example is derived from [Microsoft Documentation](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/windows-sandbox-sample-configuration)*
#### .wsb Config File Breakdown

`<Configuration>`

Top-level element — everything goes inside here.

---

  ```xml
  <VGpu>Disable</VGpu>
  ```

- Controls **virtual GPU (graphics acceleration)**.
    
- `Disable` = no GPU passthrough → the sandbox uses software rendering.  
    Useful if you want minimal hardware exposure or to reduce attack surface.
    

---

  ```xml
  <Networking>Disable</Networking>
  ```

- Disables **network access** inside the sandbox.
    
- No internet, no LAN, no communication → isolates the environment more tightly.  
    (Good for analyzing files that might attempt network connections).
    

---

  ```xml
    <MappedFolders>
    <MappedFolder>
      <HostFolder>C:\Users\Public\Downloads</HostFolder>
      <SandboxFolder>C:\temp</SandboxFolder>
      <ReadOnly>true</ReadOnly>
    </MappedFolder>
  </MappedFolders>
  ```
`

- **Maps a folder from the host into the sandbox.**
    
    - `HostFolder`: The folder on your actual Windows system → here, your `Downloads`.
    - `SandboxFolder`: Where that folder will appear inside the sandbox → `C:\temp`.
    - `ReadOnly`: `true` = sandbox can only **read** files from the mapped folder, cannot modify them.
        

⚠️ This is a **safe way** to test potentially suspicious files: you can open them in the sandbox without risking changes to your real system.

---

  ```xml
  <LogonCommand>     
	  <Command>explorer.exe C:\temp</Command>   
  </LogonCommand>
  ```

- **Runs a command automatically** when the sandbox starts.
    
- Here, it opens Windows Explorer focused on `C:\temp` (the mapped `Downloads` folder).
    
- That means as soon as Sandbox launches, you’ll immediately see the files you shared.
    

---

``` xml
</Configuration>
```

Closes the file.
